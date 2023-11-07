<script setup>

</script>
<template>
    <header>
        <nav>
            <div class="logo">
                <img src="https://www.youbike.com.tw/region/assets/images/logo.svg" alt="">
            </div>
            <ul>
                <li>使用說明</li>
                <li>收費方式</li>
                <li>站點資訊</li>
                <li>最新消息</li>
                <li>活動專區</li>
            </ul>
            <div class="btn">
                <button>登入</button>
            </div>
        </nav>
    </header>
</template>
<style lang='scss'>
* {
    margin: 0;
    padding: 0;
    list-style: none;
}

header {
    // width: 1200px;
    // margin: 30px auto 0;
    padding: 30px 200px;
    border-bottom: 1px solid #aaa;

    nav {
        display: flex;
        align-items: center;

        .logo {
            width: 100px;
            // height: 100px;

            img {
                width: 100%;
                vertical-align: bottom;
            }
        }

        ul {
            display: flex;

            li {
                // border: 1px solid red;
                padding: 10px 20px;
                margin: 0 20px;
                color: #677510;
                font-size: 18px;
                font-weight: bold;
                cursor: pointer;

                &:hover {
                    color: #B5CC22;
                }
            }
        }

        .btn {
            margin-left: auto;

            button {
                background-color: #B5CC22;
                border: none;
                width: 85px;
                height: 40px;
                border-radius: 100px;
                color: #fff;
                font-size: 18px;
                cursor: pointer;
            }

        }
    }
}
</style>