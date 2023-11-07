<script setup>
import { ref, computed, watch, onMounted, onBeforeMount } from "vue";

const bikeData = ref();
const areaList = ref([]);
const id = ref(0);
const area = ref([]);
const allCheck = ref(true);
const searchArea = ref('');

const areaFilter = computed(() => {
    return bikeFilter.value.filter(el => {
        return el.ar
            .toLowerCase()
            .includes(searchArea.value.toLowerCase())
    })
})
const bikeFilter = computed(() => {
    // 篩選check是true的出來
    let selectedAreas = area.value.filter((el) => { return el.check === true })
    let filteredData = []

    // 用selectedAreas去對裡面的每一個資料做處理
    selectedAreas.forEach((selectedAreas) => {
        // 用bikeData的每一筆來篩選，回傳兩個的地區相同的那筆
        let filteredAreaData = bikeData.value.filter((data) => {
            return data.sarea === selectedAreas.title
        })
        // 把比對完的資料送進空Array
        filteredData.push(...filteredAreaData)
    })
    // 回傳出去
    return filteredData
});
const selectAllAuto = computed(() => {
    // 篩選check是false的出來
    let selectedAreas = area.value.filter((el) => { return el.check === false })
    if (selectedAreas.length === 0) {
        return allCheck.value = true
    } else {
        return allCheck.value = false
    }
    // return selectedAreas.length
});

// 全勾的狀態切換
const selectAll = () => {
    area.value.forEach((area) => {
        area.check = true
        if (allCheck.value) {
            area.check = false
        }
    });
}

// 拿bike資料
const fetchData = () => {
    fetch('https://tcgbusfs.blob.core.windows.net/dotapp/youbike/v2/youbike_immediate.json')
        .then(res => res.json())
        .then(res => {
            // console.log(res);
            bikeData.value = res
        })
};

// 監聽bikeDate有資料時，對他做處理
watch(bikeData, (newVal, oldVal) => {
    // console.log(newVal);
    //把各區名稱放進areaList
    let arr = []
    bikeData.value.forEach(element => {
        if (arr.indexOf(element.sarea) === -1) {

            arr.push(element.sarea)
        }
    });
    // console.log(arr);
    areaList.value = arr
    // 重新包裝areaList後放進area
    let arr2 = []
    for (let i = 0; i < areaList.value.length; i++) {
        let obj = {
            id: id.value,
            title: areaList.value[i],
            check: true
        }
        arr2.push(obj)
        id.value++
    }
    // console.log(arr2);
    area.value = arr2
})
onMounted(() => {
    fetchData()
});
</script>
<template>
    <div class="wrap">
        <h1>站點資訊</h1>
        <!-- 選縣市 -->
        <div class="search">
            <select name="" id="">
                <option value="">台北市</option>
                <option value="">新北市</option>
                <option value="">桃園市</option>
            </select>
            <input type="text" placeholder="搜尋站點" v-model="searchArea">
        </div>
        <!-- 選全部區域 -->
        <div class="checkAll">
            <input type="checkbox" id="all" @click="selectAll" v-model="allCheck">
            <label for="all">全部勾選</label>
        </div>
        <!-- 選個別區域 -->
        <div class="checkArea">
            <div class="check" v-for='(area, index) in area'>
                <input type="checkbox" :id="area.id" v-model="area.check">
                <label :for="area.id">{{ area.title }}</label>
            </div>
        </div>
    </div>

    <!-- 所有bike資料 -->
    <div class="info">
        <div class="title">
            <p>縣市</p>
            <p>區域</p>
            <p>站點名稱</p>
            <p>可借車輛</p>
            <p>可還空位</p>
        </div>
        <ul class="content">
            <li v-for="bike in areaFilter">
                <div class="city">
                    <p>台北市</p>
                </div>
                <div class="area">
                    <p>{{ bike.sarea }}</p>
                </div>
                <div class="staName">
                    <p>{{ bike.ar }}</p>
                </div>
                <div class="borrowCount">
                    {{ bike.tot }}
                </div>
                <div class="returnCount">
                    {{ bike.sbi }}
                </div>
            </li>

        </ul>
    </div>
</template>
<style lang='scss'>
.wrap {
    width: 1500px;
    margin: auto;

    h1 {
        font-size: 24px;
        font-weight: bold;
        color: #B5CC22;
        margin: 50px 0;
    }

    .search {
        // border: 1px solid red;
        margin-bottom: 20px;

        select {
            width: 175px;
            height: 40px;
            background-color: #F6F6F6;
            padding: 5px 10px;
            font-size: 18px;
            border-radius: 8px;
            border: none;
            margin-right: 30px;
        }

        input {
            width: 250px;
            height: 30px;
            border-radius: 8px;
            border: none;
            padding: 5px 10px;
            font-size: 18px;
            background-color: #F6F6F6;
        }
    }

    .checkAll {
        // border: 1px solid red;
        display: flex;
        align-items: center;

        input {

            margin: 10px;
            width: 20px;
            height: 20px;
        }
    }

    .checkArea {
        // border: 1px solid red;
        width: 800px;
        margin-bottom: 50px;
        display: grid;
        grid-template-columns: repeat(4, 1fr);
        grid-template-rows: repeat(1, 1fr);
        // gap: 10px;

        .check {
            // border: 1px solid red;
            display: flex;
            align-items: center;

            input {
                margin: 10px;
                width: 20px;
                height: 20px;
            }
        }
    }
}

.info {
    width: 1500px;
    // height: 800px;
    margin: 0 auto 100px;
    text-align: center;
    // overflow: scroll;


    .title {
        display: flex;
        background-color: #B5CC22;
        border-radius: 28px 28px 0 0;

        p {
            // border: 1px solid red;
            width: 100%;
            padding: 20px 50px;
            color: #fff;
        }
    }

    .content {
        height: 500px;
        overflow-y: scroll;

        li {
            display: flex;

            div {
                width: 100%;
                padding: 20px 50px;
            }
        }

        li:nth-child(even) {
            background-color: #eee;
        }

        li:last-child {
            border-radius: 0 0 28px 28px;

        }

    }
}
</style>