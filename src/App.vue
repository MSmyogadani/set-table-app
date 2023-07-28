<script setup>
  import { ref, onMounted, watch } from 'vue'
  import Button from './components/Button.vue'
  import Input from './components/InputText.vue'

  const tableList = ref([])
  const tableCount = ref(0)
  const randomTableNum = ref()
  const isShowTable = ref(true)

  function setLocalStrage() {
    localStorage.setItem('tableList', JSON.stringify(tableList.value))
  }

  // テーブルの追加
  function addTable() {
    tableList.value.push({ number: ++tableCount.value, seatCount: 0 })
    setLocalStrage()
  }

  // テーブルの削除
  function delTable() {
    tableList.value = tableList.value.filter(table => table.number !== tableCount.value)
    tableCount.value--
    setLocalStrage()
  }

  // 座席数の設定
  function setSeatCount(event) {
    tableList.value.find((table, index) => {
      if (table.number === event.number) {
        tableList.value.splice(index, 1, {
          number: table.number,
          seatCount: parseInt(event.seatCount)
        })
      }
    })
    setLocalStrage()
  }

  watch(tableCount, () => {
    if (tableCount.value < 0) {
      tableCount.value = 0
    }
  })

  onMounted(() => {
    const storageDataTableList = localStorage.getItem('tableList')
    if (storageDataTableList !== null) {
      tableList.value = JSON.parse(storageDataTableList)
      tableCount.value = JSON.parse(storageDataTableList).length
    }
  })

  // テーブル決め
  function setTable() {
    if (tableCount.value === 0 || !tableList.value.length) {
      return false;
    }
    const endTable = tableList.value.filter(table => table.seatCount === 0).map(table => table.number)
    while (true) {
      if (endTable.length === tableCount.value) {
        break
      }
      const random = Math.floor(Math.random() * tableCount.value) + 1
      
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
        break
      }
    }
    setLocalStrage()
  }

  const btnTableAdd = 'テーブルを追加'
  const btnTableDel = 'テーブルを削除'
  const btnRandomTableNum = 'テーブルを決める'
</script>

<template>
  <div class="container-fluid w-75 mx-auto text-center">
      <div class="top mt-5 text-center">
        <div class="d-md-flex d-grid gap-2 justify-content-center justify-content-lg-between mb-3 top-item-block">
          <div class="d-flex order-1 order-lg-1 justify-content-center d-grid gap-3">
            <Button :btnName="btnTableAdd" @clickEvent="addTable"><i class="bi bi-plus-circle-fill ms-2"></i></Button>
            <Button :btnName="btnTableDel" @clickEvent="delTable"><i class="bi bi-dash-circle-fill ms-2"></i></Button>
          </div>
          <div class="form-check form-switch d-flex align-items-center p-0 order-2 order-lg-3">
            <input
              class="form-check-input mx-4 my-0"
              :class="{ 'bg-info border-info': isShowTable, 'bg-white border-secondary': !isShowTable }"
              type="checkbox"
              id="switch_1"
              name="switch_1"
              style="transform: scale(1.5); box-shadow: none;"
              @input="isShowTable = !isShowTable"
              checked
            >
            <label class="form-check-label" for="switch_1">テーブルリストを表示</label>
          </div>
        </div>
        <div v-show="isShowTable" class="row overflow-auto" style="height: 250px;">
          <div class="col-lg-2 col-6 my-3" v-for="table in tableList">
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
  @media (max-width: 767px) {
    .open-seats {
      margin-top: 10px;
    }
  }
</style>
