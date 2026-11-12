<template>
  <DestinationLayout v-bind="page" />
</template>

<script>
import DestinationLayout from "./DestinationLayout.vue";
import hero from "@/assets/carousel/Gallefort.jpg";

export default {
  name: "GallePage",
  components: { DestinationLayout },
  data() {
    return {
      page: {
        name: "Galle",
        region: "South coast",
        tagline: "Walk the old Dutch fort ramparts at sunset.",
        hero,
        intro: [
          "Galle Fort is a living town inside thick sea walls, first built by the Portuguese and greatly strengthened by the Dutch. Today its cobbled lanes hold boutique hotels, cafes, galleries, churches and mosques side by side.",
          "The fort is a UNESCO World Heritage Site, and the best way to enjoy it is on foot. Walk the ramparts at golden hour, then explore the beaches and jungle coves that line the coast on either side.",
        ],
        facts: [
          { icon: "location_city", label: "Known for", value: "Dutch fort town" },
          { icon: "military_tech", label: "Status", value: "UNESCO World Heritage Site" },
          { icon: "schedule", label: "Suggested stay", value: "1 to 2 nights" },
          { icon: "directions_car", label: "From Colombo", value: "About 2 to 2.5 hours by road" },
        ],
        highlights: [
          { icon: "directions_walk", title: "Rampart walk", text: "Follow the fort walls around the old town, with the Indian Ocean on one side and rooftops on the other." },
          { icon: "wb_twilight", title: "Lighthouse sunset", text: "Gather near the lighthouse at the end of the day as the sky turns orange over the sea." },
          { icon: "storefront", title: "Boutique lanes", text: "Browse small galleries, jewellery shops and design stores along the quiet streets." },
          { icon: "coffee", title: "Fort cafes", text: "Slow lunches and sweet treats in restored colonial buildings, many with leafy courtyards." },
          { icon: "beach_access", title: "Unawatuna and Jungle Beach", text: "Swim and snorkel at the calm beaches just along the coast from the fort." },
          { icon: "history_edu", title: "Heritage museums", text: "Learn about the Dutch and maritime history of the region in the fort's small museums." },
        ],
        bestTime: {
          months: "Dec – Mar",
          note: "Calm seas and dry weather along the south and west coasts, which suits beaches and relaxed fort walks.",
        },
        itinerary: [
          { time: "9:00 am", title: "Fort lanes and cafes", text: "Start with a slow breakfast, then wander the streets before the heat builds." },
          { time: "12:30 pm", title: "Lunch inside the walls", text: "A long lunch in a colonial courtyard, followed by a rest." },
          { time: "3:30 pm", title: "Beach time", text: "Head out to Unawatuna or Jungle Beach for a swim." },
          { time: "5:30 pm", title: "Ramparts at sunset", text: "Walk back along the walls and watch the sun go down by the lighthouse." },
        ],
        tips: [
          "The fort is compact and best on foot, with comfortable shoes and a hat.",
          "Midday is hot, so plan walking for morning and late afternoon.",
          "Many fort hotels are small, so we recommend booking early for peak months.",
          "Galle pairs well with Mirissa for whale watching and the rest of the south coast.",
        ],
      },
    };
  },
};
</script>
