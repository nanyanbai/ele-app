<template>
    <div  class="city">
        <div class="search_wrap">
            <div class="search">
                <i class="fa fa-search"></i>
                <input  v-model="city_val" type="text" placeholder="输入城市名">
            </div>
            <button >取消</button>
        </div>
        <div class="location">
            <Location  :address="city"/>
        </div>
    </div>
</template>

<script>
import Location from "../components/Location";
    export  default {
        name: "City",
        data() {
            return {
                city_val: "",
            }
        },
        computed: {
            city() {
                return (
                    this.$store.getters.location.addressComponent.city ||
                    this.$store.getters.location.addressComponent.province
                );
            }
        },
        components: {
            Location
        }
    }
</script>

<style scoped>
.city {
  width: 100%;
  height: 100%;
  overflow: auto;
  box-sizing: border-box;
  padding-top: 45px;
}
.search_wrap {
  position: fixed;
  top: 0;
  height: 55px;
  width: 100%;
  background: #fff;
  box-sizing: border-box;
  padding: 10px 16px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.search {
  background-color: #eee;
  border-radius: 10px;
  line-height: 32px;
  width: 85%;
  box-sizing: border-box;
  padding: 0 16px;
}

.search input {
  background: #eee;
  outline: none;
  border: none;
  margin-left: 5px;
}
.search_wrap button {
  outline: none;
  border: 0 none;
  height: 30px;
  color: #009eef;
  background-color: transparent
}

.location {
  background: #fff;
  padding: 8px 16px;
  height: 65px;
  box-sizing: border-box;
}

.search_list {
  background: #fff;
  padding: 5px 16px;
}
.search_list li {
  padding: 10px;
  border-bottom: 1px solid #eee;
}
</style>
