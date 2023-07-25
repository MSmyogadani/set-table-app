<script setup>
  import { ref, reactive, watch } from 'vue'
  import Button from './components/Button.vue'
  import Input from './components/InputText.vue'

  const tableList = ref([])
  const sumTable = ref(0)
  const randomTableNum = ref()

  // テーブルの追加
  function addTable() {
    tableList.value.push({ number: ++sumTable.value, seatCount: 0 })
  }

  // テーブルの削除
  function delTable() {
    tableList.value = tableList.value.filter(table => table.number !== sumTable.value)
    sumTable.value--
  }

  // 座席数の設定
  function setSeatCount(event) {
    tableList.value.find((table, index) => {
      if (table.number === event.number) {
        tableList.value.splice(index, 1, {
          number: table.number,
          seatCount: event.seatCount
        })
      }
    })
    console.log(JSON.stringify(tableList))
  }

  watch(sumTable, () => {
    if (sumTable.value < 0) {
      sumTable.value = 0
    }
  })

  // テーブル決め
  function setTable() {
    const endTable = tableList.value.filter(table => table.seatCount === 0).map(table => table.number)
    
    while (true) {
      if (endTable.length === sumTable) {
        break
      }
      const random = Math.floor(Math.random() * sumTable.value) + 1
      if (!endTable.includes(random)) {
        randomTableNum.value = random
        tableList.value.find((table, index) => {
          if (table.number === random) {
            tableList.value.splice(index, 1, {
              number: table.number,
              seatCount: --table.seatCount
            })
          }
        })
        console.log(JSON.stringify(tableList))
        break
      }
    }
  }

  const btnTableAdd = 'テーブルを追加'
  const btnTableDel = 'テーブルを削除'
  const btnRandomTableNum = 'テーブルを決める'
</script>

<template>
  <div class="container-fluid w-75 mx-auto text-center">
      <div class="top mt-5 text-center">
        <div class="d-flex justify-content-start mb-3 position-relative">
          <div class="d-flex d-grid gap-3">
            <Button :btnName="btnTableAdd" @clickEvent="addTable"/>
            <Button :btnName="btnTableDel" @clickEvent="delTable"/>
          </div>
          <h1 class="text-secondary position-absolute top-50 start-50 translate-middle">残り席数</h1>
        </div>
        <div class="row">
          <div class="col-2 my-3" v-for="table in tableList">
            <Input :tableNumber="table" @inputEvent="setSeatCount($event)" />
          </div>
        </div>
      </div>

      <div class="main">
        <span class="h5 text-secondary">あなたのテーブル</span>
        <div class="shadow p-3 mt-2 mb-3 bg-body rounded m-auto d-flex flex-column justify-content-center align-items-center" style="width: 200px; height: 200px;">
          <h1 class="text-secondary fw-bold" style="transform: scale(3)">{{ randomTableNum }}</h1>
        </div>
        <Button :btnName="btnRandomTableNum" @clickEvent="setTable" />
      </div>
  </div>
</template>

<style scoped>

</style>
