<template>
  <DestinationLayout v-bind="page" />
</template>

<script>
import DestinationLayout from "./DestinationLayout.vue";
import hero from "@/assets/carousel/Seegiriya.png";

export default {
  name: "SigiriyaPage",
  components: { DestinationLayout },
  data() {
    return {
      page: {
        name: "Sigiriya",
        region: "Cultural Triangle",
        tagline: "Climb the ancient rock fortress at sunrise and look out over jungle and lakes.",
        hero,
        intro: [
          "Sigiriya, often called Lion Rock, is a huge column of rock rising out of the jungle in the heart of the Cultural Triangle. In the 5th century a king built a palace on its summit, with water gardens, frescoes and a mirror wall along the way up.",
          "It is a UNESCO World Heritage Site and one of the most memorable climbs on the island. Around it you will find cave temples, quiet village lanes and wildlife parks, which makes Sigiriya a natural base for exploring the ancient cities.",
        ],
        facts: [
          { icon: "landscape", label: "Region", value: "Cultural Triangle" },
          { icon: "military_tech", label: "Status", value: "UNESCO World Heritage Site" },
          { icon: "schedule", label: "Suggested stay", value: "1 to 2 nights" },
          { icon: "directions_car", label: "From Colombo", value: "About 4 to 5 hours by road" },
        ],
        highlights: [
          { icon: "landscape", title: "Sigiriya Rock climb", text: "Climb the stairways to the summit ruins for wide views over forest and lakes. Go at opening time for cooler air and softer light." },
          { icon: "water_drop", title: "Water gardens", text: "Walk through the symmetrical pools, fountains and terraces at the foot of the rock, among the oldest landscaped gardens in Asia." },
          { icon: "photo_camera", title: "Pidurangala Rock", text: "A shorter, rougher climb to a neighbouring rock with a classic view of Sigiriya itself, popular for sunrise and sunset." },
          { icon: "temple_buddhist", title: "Dambulla cave temples", text: "Painted caves filled with Buddha statues, a short drive away and an easy pairing with a Sigiriya morning." },
          { icon: "pets", title: "Elephant safari", text: "Jeep safaris in nearby national parks can bring you close to herds of wild elephants, especially in the drier months." },
          { icon: "restaurant", title: "Village cooking", text: "Join a village home for a rice-and-curry cooking session, with a short bullock-cart or tuk-tuk ride through the paddy fields." },
        ],
        bestTime: {
          months: "Jan – Sep",
          note: "Generally the drier months for the Cultural Triangle. Start early on any day, as the midday heat on the rock can be strong.",
        },
        itinerary: [
          { time: "6:00 am", title: "Early start at the rock", text: "Arrive as the site opens, walk through the water gardens and begin the climb before the crowds and the heat." },
          { time: "10:00 am", title: "Breakfast and rest", text: "Back down for a relaxed breakfast and a cool drink near your hotel." },
          { time: "2:00 pm", title: "Village and countryside", text: "A gentle afternoon with a village visit, a cooking class or a paddy-field ride." },
          { time: "4:30 pm", title: "Pidurangala at golden hour", text: "Climb the neighbouring rock for a view back across to Sigiriya in the evening light." },
        ],
        tips: [
          "Wear comfortable shoes with good grip, and carry water for the climb.",
          "Dress respectfully for temple stops: shoulders and knees covered.",
          "Opening hours and ticket prices change, so we confirm them when we plan your trip.",
          "The hot, bright hours are best spent resting, so plan climbs for early morning.",
        ],
      },
    };
  },
};
</script>
