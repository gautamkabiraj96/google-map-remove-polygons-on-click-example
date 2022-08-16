<template>
  <div>
    <div class="header-margins">
      <h3>Remove polygons on click (Google Maps)</h3>
      <div class="np-polygon-length">
        Total polygons: {{ polygonsDataSetMutable.length }}
      </div>
    </div>
    <div id="polygon-label-map" class="map-margins"></div>
    <div
      v-for="(polygonData, pIndex) in polygonsDataSetMutable"
      :key="pIndex"
      class="header-margins"
    >
      <div class="np-polygon-data">
        <div>id: {{ polygonData.id }}</div>
        <div>
          coords:
          {{ polygonData.coords }}
        </div>
      </div>
    </div>
    <small class="header-margins">www.nightprogrammer.com</small>
  </div>
</template>

<script>
import loadGoogleMapsApi from "load-google-maps-api";
import { gMapsApiKey } from "./../constants";
import { polygonCoordsSetData } from "./polygonsets";

export default {
  name: "Polygons",
  data() {
    return {
      map: null,
      polygonsDataSetMutable: polygonCoordsSetData,
      polygonShapes: [],
    };
  },
  mounted() {
    loadGoogleMapsApi({
      key: gMapsApiKey,
      libraries: ["places", "drawing", "geometry"],
    }).then(() => {
      const mapZoom = 12;
      const { google } = window;
      const mapOptions = {
        zoom: mapZoom,
        mapTypeId: google.maps.MapTypeId.HYBRID,
        center: new google.maps.LatLng({ lat: 28.680272, lng: 76.5314777 }),
        mapTypeControl: true,
        streetViewControl: false,
        mapTypeControlOptions: {
          position: google.maps.ControlPosition.BOTTOM_LEFT,
        },
      };
      this.map = new google.maps.Map(
        document.getElementById("polygon-label-map"),
        mapOptions
      );

      const polygonCoordsSet = this.polygonsDataSetMutable;

      const tempBounds = new google.maps.LatLngBounds();

      for (let i = 0; i < polygonCoordsSet.length; i++) {
        polygonCoordsSet[i].coords.forEach((coord) => {
          const BoundLatLng = new google.maps.LatLng({
            lat: parseFloat(coord.lat),
            lng: parseFloat(coord.lng),
          });
          tempBounds.extend(BoundLatLng);
        });
      }

      this.polygonShapes = [];
      const fillColors = ["#ff0000", "#00ff00", "#00ff00"];

      for (let i = 0; i < polygonCoordsSet.length; i++) {
        const polygonShapeContructor = new google.maps.Polygon({
          paths: polygonCoordsSet[i].coords,
          strokeColor: "#ffffff",
          map: this.map,
          strokeWeight: 3,
          fillColor: fillColors[i],
          fillOpacity: 0.2,
          zIndex: 99999,
        });

        this.polygonShapes.push({
          id: i,
          polygon: polygonShapeContructor,
        });

        this.polygonShapes.forEach((polygonShape, i) => {
          google.maps.event.addListener(polygonShape.polygon, "click", () => {
            polygonShape.polygon.setMap(null);
            this.polygonsDataSetMutable = this.polygonsDataSetMutable.filter(
              (pData, pIndex) => pData.id !== polygonShape.id
            );
          });
        });
        this.map.setZoom(7);
      }
    });
  },
};
</script>