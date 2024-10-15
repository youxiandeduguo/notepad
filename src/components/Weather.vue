<template>
  <div class="weather_frame">
    <img :src="getImageUrl()" alt="天气" />
    <div>
      <span>{{ weatherInfo.city }}</span>
      <span>{{ weatherInfo.temperature }}°C</span>
    </div>
  </div>
</template>

<script lang="ts">
import axios from 'axios';
import { onMounted } from 'vue';
import { ref } from 'vue';
export default {
  name:"Weather"
};
</script>

<script setup lang="ts">
  
    let apiKey='fecb1a632f6114b6d36591d54799bcc8'
    let weatherInfo=ref({
      city:'',
      weather:'',
      temperature:''
    })
    const iconWeatherMap = {
      '风': ['有风', '平静', '微风', '和风', '清风', '强风/劲风', '疾风', '大风', '烈风', '风暴', '狂爆风', '飓风', '热带风暴', '龙卷风'],
      '多云': ['少云', '晴间多云', '多云'],
      '雪': ['雪', '阵雪', '小雪', '中雪', '大雪', '暴雪', '小雪-中雪', '中雪-大雪', '大雪-暴雪', '冷'],
      '雾': ['浮尘', '扬沙', '沙尘暴', '强沙尘暴', '雾', '浓雾', '强浓雾', '轻雾', '大雾', '特强浓雾'],
      '晴': ['晴', '热'],
      '雨夹雪': ['雨雪天气', '雨夹雪', '阵雨夹雪'],
      '雨': ['阵雨', '雷阵雨', '雷阵雨并伴有冰雹', '小雨', '中雨', '大雨', '暴雨', '大暴雨', '特大暴雨', '强阵雨', '强雷阵雨', '极端降雨', '毛毛雨/细雨', '雨', '小雨-中雨', '中雨-大雨', '大雨-暴雨', '暴雨-大暴雨', '大暴雨-特大暴雨', '冻雨'],
      '阴': ['阴', '霾', '中度霾', '重度霾', '严重霾', '未知']
    }

    onMounted(()=>{
      getWeather()
      getImageUrl()
    })
    async function getWeather() {
      try {
        // 通过高德API获取城市天气
        const response = await axios.get(
          'https://restapi.amap.com/v3/weather/weatherInfo',
          {
            params: {
              key: apiKey,
              city: '370215',
            },
          }
        );

        const data = response.data;
        // console.log(data)
        if (data.status == '1') {
          const weatherData = data.lives[0];
          weatherInfo.value = {
            city: weatherData.city,
            weather: weatherData.weather,
            temperature: weatherData.temperature,
          };
        } else {
          alert('无法获取该城市的天气信息');
        }
      } catch (error) {
        console.error('获取天气信息失败:', error);
        alert('获取天气信息失败');
      }
    }
let path = ref('')
const getImageUrl = () => {
  for (const weatherKey in iconWeatherMap) {
  
    const weatherNames = iconWeatherMap[weatherKey]
    const findWeatherItem = weatherNames.find(name => weatherInfo.value.weather === name)

    if (findWeatherItem) {
      path.value=weatherKey
      break
    }
  
  }

  return new URL(`../image/icon/weather/${path.value}.png`, import.meta.url).href;
}
function print(){
  // console.log(weatherInfo.value.weather)
  console.log(path)
} 


</script>
<style scoped>
  .weather_frame {
  background: linear-gradient(135deg, #e0c3fc 0%, #8ec5fc 100%);
  padding: 20px;
  border-radius: 15px;
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.15);
  display: flex;
  align-items: center;
  justify-content: space-between;
  max-width: 200px;
  margin: 20px auto;
  color: #2c3e50;
}

.weather_frame span {
  font-size: 1.2rem;
  font-weight: 500;
  color: #34495e;
}

.weather_frame img {
  width: 100px;
  height: 100px;
  object-fit: contain;
}

.weather_frame span:first-child {
  font-size: 2.5rem;
  font-weight: 600;
}

.weather_frame span:last-child {
  font-size: 2.2rem;
  font-weight: 400;
  color: #2c3e50;
  margin-left: 10px;
}
</style>
