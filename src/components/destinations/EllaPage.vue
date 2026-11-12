<template>
  <DestinationLayout v-bind="page" />
</template>

<script>
import DestinationLayout from "./DestinationLayout.vue";
import hero from "@/assets/carousel/Ninearch.jpg";

export default {
  name: "EllaPage",
  components: { DestinationLayout },
  data() {
    return {
      page: {
        name: "Ella",
        region: "Hill country",
        tagline: "Train rides, tea hills and the famous Nine Arch Bridge.",
        hero,
        intro: [
          "Ella is a small, relaxed town set in a green gap in the hill country, with views that drop away towards the southern plains. It is known for its walking trails, waterfalls and the colourful trains that wind through the tea estates.",
          "Days here are easy: a morning hike, a long lunch at a cafe with a view, and an afternoon watching the train cross the Nine Arch Bridge. It is also a good place to slow down between the busier stops on your route.",
        ],
        facts: [
          { icon: "terrain", label: "Region", value: "Hill country" },
          { icon: "train", label: "Signature ride", value: "Scenic hill-country train" },
          { icon: "schedule", label: "Suggested stay", value: "2 nights" },
          { icon: "directions_car", label: "From Colombo", value: "About 7 hours by road" },
        ],
        highlights: [
          { icon: "train", title: "Nine Arch Bridge", text: "A stone-and-brick railway viaduct set among jungle and tea. Time your visit with a passing train for the classic photograph." },
          { icon: "hiking", title: "Little Adam's Peak", text: "An easy, rewarding walk through tea fields to a summit with wide views over the valley." },
          { icon: "landscape", title: "Ella Rock", text: "A longer, steeper hike for a higher viewpoint. Best with a local guide, as the trail can be confusing." },
          { icon: "water_drop", title: "Ravana Falls", text: "A wide waterfall beside the road to the south of town, a simple stop that looks best after rain." },
          { icon: "park", title: "Tea estate visit", text: "See how tea is picked and processed at a nearby factory, then taste a fresh cup with the hills in view." },
          { icon: "coffee", title: "Cafe terraces", text: "Spend an afternoon at a hillside cafe. Ella has a relaxed, traveller-friendly food scene." },
        ],
        bestTime: {
          months: "Jan – Apr",
          note: "Clearer skies for views and hikes across the hill country. Mornings are usually the clearest, and mist often rolls in by afternoon.",
        },
        itinerary: [
          { time: "7:00 am", title: "Hike Little Adam's Peak", text: "A gentle early walk with views across the valley while the air is still cool." },
          { time: "11:00 am", title: "Breakfast with a view", text: "Settle into a terrace cafe and take your time." },
          { time: "1:30 pm", title: "Tea estate stop", text: "Visit a plantation and factory, then walk among the tea bushes." },
          { time: "4:00 pm", title: "Nine Arch Bridge", text: "Arrive before the train and watch it cross the arches in the afternoon light." },
        ],
        tips: [
          "Train tickets on the scenic route can sell out, so we help you book ahead.",
          "Bring a light rain jacket, since showers can arrive quickly in the hills.",
          "Stand well back from the track at the bridge and watch for trains.",
          "Nights can be cool, so pack a warm layer.",
        ],
      },
    };
  },
};
</script>
