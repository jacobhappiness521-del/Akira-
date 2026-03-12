# 🎌 Anime Chatbot Dataset & Corpus Package
## Complete Training Data for an Expert Anime Chatbot

---

## 📦 Package Contents

| File | Type | Description |
|------|------|-------------|
| `anime_corpus_part1.yml` | ChatterBot YAML Corpus | Core anime knowledge — series overviews, characters, plots, power systems |
| `anime_corpus_part2.yml` | ChatterBot YAML Corpus | Debates, rankings, recommendations, hidden gems, comparisons |
| `anime_dataset.json` | Custom JSON Dataset | 50 structured Q&A entries with metadata for fine-tuning/RAG |
| `README.md` | Documentation | This file — usage instructions and structure guide |

---

## 🎯 Anime Covered

### Primary Focus (Deep Coverage)
| Anime | Topics Covered |
|-------|---------------|
| Spirited Away | Characters, themes, real-world inspirations, symbolism |
| Solo Leveling | Jin-Woo, Shadow Army, Monarchs, System, power analysis |
| JoJo's Bizarre Adventure | All parts, Stands, Dio, Giorno, GER, Hamon, music trivia |
| Akame ga Kill | Night Raid, Esdeath, Imperial Arms, moral complexity, deaths |
| Hell's Paradise | Gabimaru, Tansen, Tao system, island lore, Sagiri |
| Demon Slayer | Breathing Styles, Hashira, Muzan, Rengoku, Zenitsu, Corps history |
| Frieren | Frieren, Fern, Himmel, themes of grief and time, magic system |
| Jujutsu Kaisen | Gojo, Sukuna, Domain Expansion, Cursed Energy, Shibuya Incident |
| Blue Lock | Isagi, Rin, Ego, Meta-Vision, sports philosophy comparison |
| Blue Box | Overview and context |
| One Punch Man | Saitama, Genos, Garou, power philosophy, limiter |
| My Hero Academia | OFA, AFO, Todoroki, Shigaraki, hero society themes |
| Hunter x Hunter | Nen system, Killua, Gon, Meruem, Chimera Ant arc |
| Spy x Family | Anya, Loid, Yor, family dynamics, emotional moments |
| Tokyo Ghoul | Kaneki, Kagune, Arima, white hair symbolism, manga vs anime |
| Kaiju No. 8 | Kafka, Defense Force, world-building |
| Sakamoto Days | Sakamoto, action comedy, character analysis |
| Apothecary Diaries | Maomao, mysteries, court intrigue, historical setting |
| Classroom of the Elite | Ayanokoji, White Room, point system, light novel vs anime |
| Black Clover | Asta, Anti-Magic, Liebe, Yami, Black Bulls |
| Tougen Anki | Shiki, Kishin clans, mythology, supernatural lore |
| Daily Life of Immortal King | Wang Ling, cultivation system, overpowered comedy |

### Extended Coverage
- Attack on Titan, Fullmetal Alchemist Brotherhood, Vinland Saga
- Chainsaw Man, Re:Zero, Overlord, TenSura, Naruto, Bleach
- One Piece, Dragon Ball Z, Death Note, Steins;Gate
- No Game No Life, Berserk, Mushoku Tensei, and more

---

## 🛠️ How to Use These Files

### 1. ChatterBot Integration (YAML Corpus Files)

```python
from chatterbot import ChatBot
from chatterbot.trainers import ChatterBotCorpusTrainer

# Initialize chatbot
chatbot = ChatBot(
    'AnimeChatBot',
    storage_adapter='chatterbot.storage.SQLStorageAdapter',
    database_uri='sqlite:///anime_chatbot.sqlite3',
    logic_adapters=[
        {
            'import_path': 'chatterbot.logic.BestMatch',
            'default_response': "Hmm, I'm not sure about that! Try asking about a specific anime, character, or power system!",
            'maximum_similarity_threshold': 0.65
        }
    ]
)

# Train on custom corpus files
trainer = ChatterBotCorpusTrainer(chatbot)
trainer.train("anime_corpus_part1.yml")
trainer.train("anime_corpus_part2.yml")

# Optional: Also train on ChatterBot's built-in English corpus for general conversation
# trainer.train("chatterbot.corpus.english")

print("Training complete!")

# Test your chatbot
response = chatbot.get_response("Tell me about Frieren")
print(response)
```

---

### 2. Custom JSON Dataset — RAG (Retrieval-Augmented Generation)

Use the JSON dataset to build a retrieval system that feeds context to an LLM:

```python
import json
import numpy as np
from sentence_transformers import SentenceTransformer

# Load dataset
with open("anime_dataset.json", "r") as f:
    data = json.load(f)

entries = data["data"]
questions = [e["question"] for e in entries]
answers = [e["answer"] for e in entries]

# Embed questions for semantic search
model = SentenceTransformer("all-MiniLM-L6-v2")
embeddings = model.encode(questions)

def find_best_answer(user_query, top_k=3):
    query_embedding = model.encode([user_query])
    similarities = np.dot(embeddings, query_embedding.T).flatten()
    top_indices = similarities.argsort()[-top_k:][::-1]
    
    results = []
    for idx in top_indices:
        results.append({
            "question": questions[idx],
            "answer": answers[idx],
            "similarity": similarities[idx],
            "anime": entries[idx]["anime"],
            "category": entries[idx]["category"]
        })
    return results

# Test
results = find_best_answer("Who is the strongest in JJK?")
for r in results:
    print(f"[{r['similarity']:.2f}] {r['question']}")
    print(r['answer'][:200], "...\n")
```

---

### 3. Fine-Tuning Format Conversion

Convert the JSON dataset to JSONL format for fine-tuning models (OpenAI, Llama, etc.):

```python
import json

with open("anime_dataset.json", "r") as f:
    data = json.load(f)

# Convert to instruction fine-tuning format
with open("anime_finetune.jsonl", "w") as out:
    for entry in data["data"]:
        record = {
            "messages": [
                {
                    "role": "system",
                    "content": "You are AKIRA, an expert anime chatbot with deep knowledge of anime series, characters, plot lines, power systems, and themes."
                },
                {
                    "role": "user",
                    "content": entry["question"]
                },
                {
                    "role": "assistant",
                    "content": entry["answer"]
                }
            ]
        }
        out.write(json.dumps(record) + "\n")

print("Fine-tuning dataset saved to anime_finetune.jsonl")
```

---

### 4. Simple Rule-Based Lookup (No ML Required)

```python
import json

with open("anime_dataset.json", "r") as f:
    data = json.load(f)

def simple_anime_bot(user_input):
    user_lower = user_input.lower()
    best_match = None
    best_score = 0
    
    for entry in data["data"]:
        # Simple keyword matching
        score = 0
        for word in user_lower.split():
            if word in entry["question"].lower():
                score += 1
            if any(word in tag.lower() for tag in entry["tags"]):
                score += 0.5
        
        if score > best_score:
            best_score = score
            best_match = entry
    
    if best_match and best_score > 0:
        return f"[{best_match['anime']}] {best_match['answer']}"
    return "I don't have specific information on that. Try asking about a character, plot, or power system!"

# Test
print(simple_anime_bot("Tell me about Gojo Satoru"))
print(simple_anime_bot("strongest demon slayer"))
```

---

## 📊 Dataset Structure

### JSON Dataset Fields

```json
{
  "id": 1,
  "category": "plot_summary",
  "anime": "Solo Leveling",
  "question": "What is the main plot of Solo Leveling?",
  "answer": "Detailed answer...",
  "difficulty": "beginner",
  "tags": ["Solo Leveling", "manhwa", "power fantasy"]
}
```

**Categories:**
- `character_info` — Character backgrounds, abilities, personalities
- `plot_summary` — Story overviews and arc summaries
- `power_systems` — Magic systems, abilities, power mechanics
- `themes` — Thematic analysis and symbolism
- `rankings` — Best-of lists and power tier discussions
- `comparisons` — Cross-anime comparisons and debates
- `recommendations` — Watch suggestions based on preferences
- `lore_deep_dive` — World-building and hidden lore
- `emotional_reactions` — Memorable/impactful moments
- `controversies` — Fan debates and divisive topics
- `trivia` — Fun facts and behind-the-scenes knowledge

**Difficulty Levels:**
- `beginner` — Accessible to anyone, no prior anime knowledge needed
- `intermediate` — Assumes basic familiarity with the series
- `advanced` — Deep lore, requires significant series knowledge

---

## 🔧 ChatterBot Corpus Format

The YAML files follow standard ChatterBot corpus format:

```yaml
categories:
  - CategoryName

conversations:
  - - "User message"
    - "Bot response"
  
  - - "Alternative question"
    - "Another response"
```

Each conversation pair can have multiple user inputs mapping to responses.

---

## 📈 Extending the Dataset

### Adding New Entries to JSON Dataset

```python
new_entry = {
    "id": 51,
    "category": "character_info",
    "anime": "New Anime Title",
    "question": "Your question here?",
    "answer": "Detailed answer here...",
    "difficulty": "intermediate",
    "tags": ["tag1", "tag2", "character_name"]
}

# Append to dataset
with open("anime_dataset.json", "r+") as f:
    data = json.load(f)
    data["data"].append(new_entry)
    data["total_entries"] = len(data["data"])
    f.seek(0)
    json.dump(data, f, indent=2)
```

### Adding to YAML Corpus

Simply add new conversation pairs to either YAML file:

```yaml
  - - "New question about anime"
    - "Detailed bot response"
```

---

## 🎌 Recommended Workflow

1. **Start with ChatterBot** using the YAML corpus for rapid deployment
2. **Add the JSON dataset** as a knowledge base for semantic search
3. **Integrate with Claude API** (as in the chatbot artifact) for dynamic responses
4. **Fine-tune a smaller model** with the JSONL format for offline use
5. **Continuously expand** by adding new anime entries as you watch more!

---

## 📝 Notes

- All answers are written in an engaging, fan-friendly tone matching the AKIRA chatbot persona
- Spoiler warnings are included where appropriate
- Power scaling entries include multiple perspectives to reflect community debate
- The dataset balances breadth (40+ anime) with depth (detailed lore entries)
- Emotional/reaction entries are designed to simulate genuine fan discussion

---

*Built for the AKIRA Anime Chatbot — Your Ultimate Anime Companion ⚡*
