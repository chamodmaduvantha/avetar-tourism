<template>
  <DestinationLayout v-bind="page" />
</template>

<script>
import DestinationLayout from "./DestinationLayout.vue";
import hero from "@/assets/carousel/Mirissa.jpg";

export default {
  name: "MirissaPage",
  components: { DestinationLayout },
  data() {
    return {
      page: {
        name: "Mirissa",
        region: "South coast",
        tagline: "Whale watching, surf and palm-lined beaches.",
        hero,
        intro: [
          "Mirissa is a laid-back beach village on the south coast, with a curved bay, palm trees and a relaxed seaside feel. It is one of the best places on the island for whale watching, and the beaches suit swimming, surfing and lazy afternoons.",
          "Evenings are easy too, with beach cafes serving fresh seafood and a friendly, low-key atmosphere. It combines well with Galle and the other south-coast stops.",
        ],
        facts: [
          { icon: "beach_access", label: "Known for", value: "Beaches and whale watching" },
          { icon: "sailing", label: "Signature trip", value: "Whale-watching boat tour" },
          { icon: "schedule", label: "Suggested stay", value: "2 nights" },
          { icon: "directions_car", label: "From Colombo", value: "About 3 hours by road" },
        ],
        highlights: [
          { icon: "sailing", title: "Whale watching", text: "Early boat trips head out to deep water in season, where blue whales and dolphins are often seen. Choose a responsible operator." },
          { icon: "beach_access", title: "Mirissa Beach", text: "A wide, golden bay with calm stretches for swimming and plenty of loungers and cafes along the shore." },
          { icon: "park", title: "Coconut Tree Hill", text: "A small headland lined with palms, popular with photographers around sunrise and sunset." },
          { icon: "surfing", title: "Surf at Weligama", text: "Nearby Weligama has gentle waves that suit beginners, with board hire and lessons on the beach." },
          { icon: "photo_camera", title: "Parrot Rock", text: "A rocky outcrop at low tide, a short walk from the main beach, with views across the bay." },
          { icon: "restaurant", title: "Seafood dinners", text: "End the day with grilled fish and cold drinks on the sand as the sun goes down." },
        ],
        bestTime: {
          months: "Dec – Mar",
          note: "Calm seas and dry weather on the south coast. Whale-watching season generally runs through the drier months, and we confirm what to expect for your dates.",
        },
        itinerary: [
          { time: "6:00 am", title: "Whale-watching trip", text: "Board early, head out to deeper water and watch for blows and tails." },
          { time: "11:00 am", title: "Late breakfast", text: "Back on shore for a relaxed meal, then a rest by the pool or beach." },
          { time: "3:30 pm", title: "Beach and Parrot Rock", text: "Swim, walk the sand and explore the rocks as the tide allows." },
          { time: "5:30 pm", title: "Sunset at Coconut Tree Hill", text: "Take photographs among the palms, then dinner by the sea." },
        ],
        tips: [
          "Take motion-sickness tablets before the boat trip if you are prone to seasickness.",
          "Bring a hat, sunscreen and a light jacket for the early boat ride.",
          "Swim only where it is safe, and heed local advice about currents.",
          "Book boats with responsible operators who keep a respectful distance from whales.",
        ],
      },
    };
  },
};
</script>
