<template>
  <DestinationLayout v-bind="page" />
</template>

<script>
import DestinationLayout from "./DestinationLayout.vue";

// Placeholder photo. Add Yala.jpg to src/assets/carousel, import it here
// (import hero from "@/assets/carousel/Yala.jpg";) and remove this line.
const hero = "https://picsum.photos/seed/lk-yala/1600/900";

export default {
  name: "YalaPage",
  components: { DestinationLayout },
  data() {
    return {
      page: {
        name: "Yala",
        region: "Southeast",
        tagline: "Early-morning safaris in leopard country.",
        hero,
        intro: [
          "Yala National Park is one of the best-known wildlife areas in Sri Lanka, a mix of scrub, lagoons, rocky outcrops and open plains near the southeast coast. It is especially famous for leopards, which can often be spotted on a good morning drive.",
          "Besides leopards, you may see elephants, sloth bears, crocodiles, water buffalo, deer and a huge variety of birds. A jeep safari at dawn is the classic way to experience it.",
        ],
        facts: [
          { icon: "pets", label: "Known for", value: "Leopards and elephants" },
          { icon: "directions_car", label: "Safari style", value: "Jeep, half or full day" },
          { icon: "schedule", label: "Suggested stay", value: "1 to 2 nights" },
          { icon: "route", label: "From Colombo", value: "About 5 to 6 hours by road" },
        ],
        highlights: [
          { icon: "pets", title: "Leopard tracking", text: "Experienced trackers read tracks and alarm calls to help find leopards. Sightings are never guaranteed, but the odds are good." },
          { icon: "forest", title: "Elephants and herds", text: "Watch elephants bathe and feed near waterholes, along with buffalo, deer and wild boar." },
          { icon: "flutter_dash", title: "Birdlife", text: "Painted storks, peacocks, eagles and many other species gather around the lagoons and wetlands." },
          { icon: "sunny", title: "Dawn safari", text: "Leave in the dark for cool air, soft light and the best chance of seeing active animals." },
          { icon: "temple_hindu", title: "Kataragama", text: "A sacred pilgrimage town nearby, visited by Buddhists, Hindus and others, with a colourful evening ceremony." },
          { icon: "spa", title: "Eco lodges", text: "Stay in tented camps or quiet lodges near the park edge for a bush atmosphere." },
        ],
        bestTime: {
          months: "Feb – Jul",
          note: "Usually the drier period, when animals gather at waterholes. The park typically closes for a short period each year, so we confirm dates before you book.",
        },
        itinerary: [
          { time: "5:30 am", title: "Safari departure", text: "Meet your jeep and tracker at the gate and enter the park as it opens." },
          { time: "10:00 am", title: "Breakfast at the lodge", text: "Return for a hearty breakfast and a rest through the hot hours." },
          { time: "3:00 pm", title: "Optional afternoon drive", text: "A second game drive, or a visit to a nearby temple or lagoon." },
          { time: "7:00 pm", title: "Dinner under the stars", text: "A relaxed meal at camp with the sounds of the bush around you." },
        ],
        tips: [
          "Wear neutral colours such as khaki, green or beige, and bring a hat and sunscreen.",
          "Binoculars make a big difference, so bring a pair if you can.",
          "Sightings depend on luck and season, and we plan with that in mind.",
          "Keep voices low and follow your tracker's guidance on the safari.",
        ],
      },
    };
  },
};
</script>
