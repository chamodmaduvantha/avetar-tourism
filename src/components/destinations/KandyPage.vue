<template>
  <DestinationLayout v-bind="page" />
</template>

<script>
import DestinationLayout from "./DestinationLayout.vue";
import hero from "@/assets/carousel/Ambuluwawa.jpg";

export default {
  name: "KandyPage",
  components: { DestinationLayout },
  data() {
    return {
      page: {
        name: "Kandy",
        region: "Central highlands",
        tagline: "Temple of the Tooth, a peaceful lake and misty mountain views.",
        hero,
        intro: [
          "Kandy is the hill capital of Sri Lanka and the last royal city of the Sinhalese kings. It sits around a calm lake, ringed by green hills, and is the spiritual centre of Buddhism on the island.",
          "The city is also a gateway to the tea country, with botanical gardens, craft villages and dance performances close by. It makes an easy first stop on a journey from the coast up into the highlands.",
        ],
        facts: [
          { icon: "temple_buddhist", label: "Known for", value: "Temple of the Tooth" },
          { icon: "military_tech", label: "Status", value: "UNESCO sacred city" },
          { icon: "schedule", label: "Suggested stay", value: "1 to 2 nights" },
          { icon: "directions_car", label: "From Colombo", value: "About 3 to 4 hours by road" },
        ],
        highlights: [
          { icon: "temple_buddhist", title: "Temple of the Tooth", text: "Sri Dalada Maligawa houses a revered Buddhist relic. Visit at puja time to see drummers and flower offerings." },
          { icon: "water_drop", title: "Kandy Lake", text: "An easy evening stroll around the lake, with the temple complex on one side and the hills beyond." },
          { icon: "park", title: "Royal Botanic Gardens", text: "Peradeniya's gardens hold orchids, giant trees and wide lawns, a pleasant half-day outside the city." },
          { icon: "landscape", title: "Ambuluwawa Tower", text: "A spiral tower on a hilltop to the south, with sweeping views and a mix of nature and religious sites around it." },
          { icon: "theater_comedy", title: "Cultural dance show", text: "An evening of traditional Kandyan dance, drumming and fire-walking in the heart of town." },
          { icon: "shopping_bag", title: "Craft and spice stops", text: "Browse wood carving, batik and spice gardens in the surrounding villages." },
        ],
        bestTime: {
          months: "Jan – Apr",
          note: "Drier, clearer weather in the hills. Kandy is also famous for its Esala Perahera procession, held in the middle of the year, so ask us about dates if you want to see it.",
        },
        itinerary: [
          { time: "9:00 am", title: "Botanic gardens", text: "Wander the lawns and orchid house while the day is cool." },
          { time: "1:00 pm", title: "Lunch with a lake view", text: "A relaxed meal near the water before the afternoon." },
          { time: "5:00 pm", title: "Temple of the Tooth", text: "Arrive for the evening puja and walk through the temple complex." },
          { time: "7:00 pm", title: "Dance performance", text: "Finish with Kandyan dance and drumming, then dinner in town." },
        ],
        tips: [
          "Dress modestly for the temple, and expect to remove shoes and hats.",
          "Traffic in central Kandy can be slow, so allow extra time for transfers.",
          "Temple entry times and ceremonies are confirmed when we plan your trip.",
          "Pair Kandy with Sigiriya before it, and Nuwara Eliya or Ella after it.",
        ],
      },
    };
  },
};
</script>
