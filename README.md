# tourism_ai_agent
# An advanced AI-powered virtual tour guide for Kashmir tourists, featuring fuzzy query matching, personalized itinerary suggestions, and up-to-date 2025 travel insights on attractions, safety, cuisine, and more.

# Enhanced Kashmir Tourism AI Agent
# This script creates a command-line chatbot with fuzzy matching, personalized recommendations, and conversation memory.
# Data sourced from 2025 travel guides (e.g., Lonely Planet, TripAdvisor, official Jammu & Kashmir tourism site).
# Author: Shah Abrar Ul Haq

import difflib  # For fuzzy matching

# Expanded knowledge base with 2025 updates
knowledge = {
    "attractions": "In 2025, top attractions include Srinagar's Dal Lake (houseboat stays and shikara rides), Pahalgam's Betaab Valley (trekking and picnics), Gulmarg's ski resorts (now with eco-friendly gondolas), Sonmarg's Thajiwas Glacier (adventure hikes), new eco-spots like Gurez Valley (offbeat camping), and cultural sites like Jamia Masjid. For families, try Tulip Garden in April blooms.",
    "best time": "Best time in 2025: April-October for mild weather (10-30°C). Peak tulip season: March-April. Summer (May-July) for festivals like Kashmir Music Fest. Autumn (September-October) for golden chinar leaves. Winter (December-February) for snow sports, but expect road closures—check advisories. Avoid monsoon if prone to landslides.",
    "safety": "As of 2025, Kashmir is safer for tourists with improved infrastructure and security in key areas like Srinagar, Gulmarg, and Pahalgam. Government reports low incidents; locals are hospitable. Stick to tourist zones, use verified apps for bookings, and monitor MEA advisories. Safe for solo women and families with precautions like group travel.",
    "tips": "Tips: Book via apps like MakeMyTrip for deals. Carry ID always. Try local Wazwan cuisine but check spice levels. Use UPI for payments. For eco-tourism, opt for sustainable homestays. Acclimatize to altitudes to avoid sickness. Rent electric vehicles in Srinagar for green travel.",
    "accommodations": "Options: Luxury houseboats on Dal Lake (~₹5000/night), budget guesthouses in Pahalgam (~₹2000), eco-resorts in Gurez (~₹3000). In 2025, new glamping sites in Sonmarg. Book early for peaks.",
    "cuisine": "Must-try: Rogan Josh, Dum Aloo, Kahwa tea. Street food like Tsot in Srinagar markets. Vegan options increasing with tourism boom.",
    "default": "Sorry, I didn't catch that. Ask about attractions, best time, safety, tips, accommodations, cuisine, or request a personalized itinerary!"
}

# Synonyms for fuzzy matching
synonyms = {
    "attractions": ["places", "sights", "visit", "spots"],
    "best time": ["when", "season", "time to go"],
    "safety": ["safe", "security", "risk"],
    "tips": ["advice", "guide", "hacks"],
    "accommodations": ["stay", "hotels", "lodging"],
    "cuisine": ["food", "eat", "dishes"]
}

# Simple memory (last topic)
memory = {"last_topic": None}

def get_closest_match(query):
    all_keys = list(knowledge.keys())[:-1]  # Exclude default
    flat_synonyms = [key] + [syn for key_syns in synonyms.values() for syn in key_syns]
    closest = difflib.get_close_matches(query, flat_synonyms, n=1, cutoff=0.6)
    if closest:
        for key, syns in synonyms.items():
            if closest[0] in syns or closest[0] == key:
                return key
    return None

def suggest_itinerary(preference):
    if "adventure" in preference:
        return "Adventure itinerary: Day 1: Gulmarg skiing. Day 2: Sonmarg trekking. Day 3: Pahalgam rafting. Pack warm gear!"
    elif "relax" in preference or "family" in preference:
        return "Relaxing/family itinerary: Day 1: Dal Lake boat ride. Day 2: Mughal Gardens picnic. Day 3: Pahalgam meadows. Focus on easy access spots."
    else:
        return "General itinerary: Mix of Srinagar culture, Gulmarg views, and local cuisine tasting."

def get_response(query):
    query_lower = query.lower()
    
    if "itinerary" in query_lower or "plan" in query_lower:
        preference = input("AI Agent: What's your preference? (e.g., adventure, relaxing, family): ")
        return suggest_itinerary(preference.lower())
    
    match = get_closest_match(query_lower)
    if match:
        memory["last_topic"] = match
        return knowledge[match]
    elif memory["last_topic"] and "more" in query_lower:
        return knowledge[memory["last_topic"]] + " Anything else?"
    else:
        return knowledge["default"]

# Main loop
print("Welcome to Enhanced Kashmir Tourism AI Agent! Ask about attractions, best time, safety, tips, accommodations, cuisine, or request an itinerary. Type 'exit' to quit.")
while True:
    user_input = input("You: ")
    if user_input.lower() == "exit":
        print("Goodbye! Safe travels to Kashmir.")
        break
    response = get_response(user_input)
    print("AI Agent: " + response)
