<template>
  <DestinationLayout v-bind="page" />
</template>

<script>
import DestinationLayout from "./DestinationLayout.vue";

// Placeholder photo. Add Nuwaraeliya.jpg to src/assets/carousel, import it here
// (import hero from "@/assets/carousel/Nuwaraeliya.jpg";) and remove this line.
const hero = "https://picsum.photos/seed/lk-nuwara/1600/900";

export default {
  name: "NuwaraEliyaPage",
  components: { DestinationLayout },
  data() {
    return {
      page: {
        name: "Nuwara Eliya",
        region: "Tea country",
        tagline: "Cool air, colonial charm and rolling estates.",
        hero,
        intro: [
          "Nuwara Eliya sits high in the central hills, surrounded by tea plantations, waterfalls and cool, misty air. Its colonial-era houses, clipped gardens and golf course have earned it the nickname Little England.",
          "It is the heart of Sri Lanka's tea country, so a visit often includes a plantation tour and a fresh cup with a view. The nearby plains and cloud forests make it a good base for walks and early starts.",
        ],
        facts: [
          { icon: "terrain", label: "Known for", value: "Tea estates, cool climate" },
          { icon: "thermostat", label: "Climate", value: "Noticeably cooler than the coast" },
          { icon: "schedule", label: "Suggested stay", value: "1 to 2 nights" },
          { icon: "directions_car", label: "From Colombo", value: "About 5 to 6 hours by road" },
        ],
        highlights: [
          { icon: "local_cafe", title: "Tea plantation tour", text: "Walk through the bushes, see the factory process and finish with a tasting of the freshly made tea." },
          { icon: "water_drop", title: "Waterfalls", text: "Several waterfalls lie close to town, and many can be seen from the road or a short walk." },
          { icon: "park", title: "Hakgala Botanical Garden", text: "Cool-climate gardens on a hillside, with rose beds, ferns and flowering trees." },
          { icon: "landscape", title: "Horton Plains", text: "A high plateau of grassland and cloud forest with a walking loop to the dramatic World's End viewpoint. Start very early." },
          { icon: "directions_walk", title: "Gregory Lake", text: "A pleasant lakeside stroll in the middle of town, with boat rides and picnic spots." },
          { icon: "train", title: "Hill-country trains", text: "Ride a scenic train through the highlands on the way to or from Ella." },
        ],
        bestTime: {
          months: "Jan – Apr",
          note: "Clearer skies for the hills and tea estates. April brings a lively local holiday season, so book early if you plan to visit then.",
        },
        itinerary: [
          { time: "5:30 am", title: "Horton Plains at dawn", text: "Reach the plateau early for the best views before clouds roll in." },
          { time: "11:00 am", title: "Warm breakfast", text: "Back in town for a hearty meal and a rest." },
          { time: "2:00 pm", title: "Tea estate visit", text: "Tour a plantation and taste tea straight from the factory." },
          { time: "5:00 pm", title: "Gregory Lake stroll", text: "A quiet walk by the lake before dinner at a colonial-style hotel." },
        ],
        tips: [
          "Pack warm layers and a rain jacket, as evenings can be chilly and misty.",
          "Horton Plains is best visited very early, before clouds cover the views.",
          "Roads are winding, so allow time and consider motion-sickness tablets.",
          "Nuwara Eliya sits well between Kandy and Ella on a hill-country route.",
        ],
      },
    };
  },
};
</script>
