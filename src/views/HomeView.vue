<template>
  <div class="home">
    <input type="text">
    <button @click="searchPlase">검색</button>
    <div style="width:100%; height: 500px" id="map"></div>
  </div>
</template>

<script>

export default {
  name: 'kakaoMap',
  data() {
    return {
      map: null, //map에 대한 설정
    }
  },
  created() {
    // if(window.kakao && window.kakao.maps) {
    //   this.loadMap();
    // } else {
    //   this.initMap();
    // }
    this.loadMap();
  },
  methods: {
    loadMap() {
      const script = document.createElement('script');
      script.src = '//dapi.kakao.com/v2/maps/sdk.js?appkey=276eb49870ad8f4fd480ef9ea8cd9d75&autoload=false';
      script.onload = () => window.kakao.maps.load(this.initMap); 
      document.head.appendChild(script);
    },
    initMap() {
      var mapContainer = document.getElementById('map'), // 지도를 표시할 div 
      mapOption = { 
          center: new window.kakao.maps.LatLng(37.5665, 126.9780), // 지도의 중심좌표
          level: 3 // 지도의 확대 레벨
      };

      // 지도를 표시할 div와  지도 옵션으로  지도를 생성합니다
      this.map = new window.kakao.maps.Map(mapContainer, mapOption); 
    },

    /**
     * 검색범위
     */
    // 키워드 검색 완료 시 호출되는 콜백함수 입니다
    placesSearchCB (data, status, pagination) {
      console.log(pagination);
        if (status === window.kakao.maps.services.Status.OK) {

            // 검색된 장소 위치를 기준으로 지도 범위를 재설정하기위해
            // LatLngBounds 객체에 좌표를 추가합니다
            var bounds = new window.kakao.maps.LatLngBounds();

            for (var i=0; i<data.length; i++) {
                this.displayMarker(data[i]);    
                bounds.extend(new window.kakao.maps.LatLng(data[i].y, data[i].x));
            }       

            // 검색된 장소 위치를 기준으로 지도 범위를 재설정합니다
            this.map.setBounds(bounds);
        } 
    },
    // 지도에 마커를 표시하는 함수입니다
    displayMarker(place) {
        
        // 마커를 생성하고 지도에 표시합니다
        var marker = new window.kakao.maps.Marker({
            map: this.map,
            position: new window.kakao.maps.LatLng(place.y, place.x) 
        });

        // 마커에 클릭이벤트를 등록합니다
        window.kakao.maps.event.addListener(marker, 'click', function() {
            // 마커를 클릭하면 장소명이 인포윈도우에 표출됩니다
            window.infowindow.setContent('<div style="padding:5px;font-size:12px;">' + place.place_name + '</div>');
            window.infowindow.open(this.map, marker);
        });
    },
    //검색하기
    searchPlase() {

    }
  },
  components: {
  }
}
</script>
