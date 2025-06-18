
# Json steams of thoughts 

##  Libraries Used

- `os`: For environment variable access
- `json`: For working with JSON data
- `typing.List`: For type hinting lists
- `dotenv`: To load environment variables from `.env`
- `pydantic`: For defining and validating data models
- `groq`: To access the LLaMA 3 model from Groq's API

---

##  Environment Setup

```python
load_dotenv()
GROQ_API_KEY = os.getenv("GROQ_API_KEY")

```
- Loads the .env file.

- Retrieves the GROQ_API_KEY.

- Raises an error if the key is missing.
  
### Groq Client Initialization

```
client = Groq(api_key=GROQ_API_KEY)
```
-Creates an authenticated Groq client to use the LLaMA 3 model.

### Prompt Builder Function

```
def build_prompt(user_input: str) -> List[dict]:
```
 - Constructs the prompt instructions for the LLaMA 3 model:

- Break the journal into individual events.

- Each must include:

- entry: number

- title: short summary (with time if mentioned)

- description: full sentence summary

- Ensures the output is valid JSON only.
  
###  LLM Response Handling
-def get_structured_entries(user_input: str) -> List[ThoughtEntry]:
*Sends prompt + user input to the Groq API.

*Uses llama3-70b-8192 model to generate structured JSON.

*Parses model output into a list of ThoughtEntry objects.

*Handles invalid JSON or exceptions gracefully.

### Main Program Logic
-def main():
    ...
* Takes journal entry from the user via input.

* Sends it to the model for structuring.

* Displays the structured output in a readable format using:

     -response.model_dump_json(indent=2)
### Sample Input
-I woke up at 6:30 in the morning and did some work during the early hours. Then, I got ready for college and left at 9:15 AM. I attended a meeting with Vikram Sir from 9:30 to 10:30 AM. After that, I worked on my project and completed some assigned tasks. Later, I had lunch and resumed my coding work, which I continued throughout the evening.
#### Structured Output
```
{
  "entries": [
    {
      "entry": 1,
      "title": "Woke up at 6:30 AM",
      "description": "Woke up at 6:30 in the morning and did some work during the early hours."
    },
    {
      "entry": 2,
      "title": "Got ready for college",
      "description": "Got ready for college and left at 9:15 AM."
    },
    {
      "entry": 3,
      "title": "Meeting with Vikram Sir",
      "description": "Attended a meeting with Vikram Sir from 9:30 to 10:30 AM."
    },
    {
      "entry": 4,
      "title": "Worked on project and completed tasks",
      "description": "Worked on my project and completed some assigned tasks."
    },
    {
      "entry": 5,
      "title": "Had lunch",
      "description": "Had lunch and resumed my coding work."
    },
    {
      "entry": 6,
      "title": "Continued coding work",
      "description": "Continued my coding work throughout the evening."
    }
  ]
}
```
### Summary
* Breaks down long journal text into short structured events.

* Uses the Groq API with LLaMA 3 for accurate language understanding.

* Outputs clean, usable JSON — ideal for timelines, logs, or reports.
