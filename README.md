# Interactive Chat with AI using Jupyter Widgets

## Overview
This Jupyter Notebook provides an interactive chat interface using `ipywidgets`. The chatbot, named **Eureka**, serves as a knowledgeable and engaging teacher, guiding users through various topics interactively. It adapts to user input, explains concepts in a structured manner, and ensures a smooth conversation flow.

## Features
- **Conversational AI:** Engages in friendly and interactive discussions.
- **Interactive UI:** Uses text input, buttons, and formatted output for a seamless experience.
- **Dynamic Topic Handling:** Can switch topics gracefully while maintaining conversation coherence.
- **Safe and Ethical AI Usage:** Follows guidelines to avoid harmful, misleading, or copyrighted content.
- **Asynchronous API Calls:** Ensures efficient communication with the AI model.

## Requirements
Ensure you have the following installed in your Jupyter environment:
- `ipywidgets`
- `asyncio`
- `openai` (or relevant AI client library)

Install missing dependencies using:
```bash
pip install ipywidgets openai
```

Create a .env file and add your OpenAi Key like : AZURE_OPENAI_API_KEY = 'YOUR KEY HERE'

## How It Works
1. The user enters a message in the text input field.
2. The chatbot processes the message and provides an appropriate response.
3. The conversation is dynamically updated in the output display.
4. The user can change topics at any time, and Eureka adapts accordingly.
5. Typing `exit` ends the conversation.

## Code Breakdown
### 1. Initializing Chat UI
```python
text_input = widgets.Text(placeholder="Type your message here...")
send_button = widgets.Button(description="Send")
output = widgets.Output()
```
- **Text Input**: Users can type their messages.
- **Send Button**: Triggers message processing.
- **Output Display**: Shows user input and AI responses.

### 2. Displaying Greeting Message
```python
display(HTML("<b>Hi! Let's chat. Type 'exit' to end the conversation.</b>"))
display(text_input, send_button, output)
```
- Displays a friendly greeting to begin the conversation.

### 3. Asynchronous AI API Call
```python
async def get_ai_response(chat_prompt, client, deployment):
    response = await asyncio.to_thread(
        client.chat.completions.create,
        model=deployment,
        messages=chat_prompt,
        max_tokens=800,
        temperature=0.7,
        top_p=0.95,
        frequency_penalty=0,
        presence_penalty=0,
        stop=None,
        stream=False
    )
    return response.choices[0].message.content.strip()
```
- Calls an AI model asynchronously to get responses.
- Uses parameters like `temperature` and `top_p` to control response variability.

### 4. Handling User Input
```python
async def handle_chat(sender):
    user_input = text_input.value.strip()
    text_input.value = ""
    
    if not user_input:
        return
    
    chat_prompt.append({"role": "user", "content": user_input})
    
    with output:
        display(HTML(f"<b>You:</b> {user_input}"))
    
    if user_input.lower() == "exit":
        with output:
            display(HTML("<b>Goodbye!</b>"))
        return
    
    assistant_response = await get_ai_response(chat_prompt, client, deployment)
    
    with output:
        display(HTML(f"<b>Eureka:</b> {assistant_response}"))
    
    chat_prompt.append({"role": "assistant", "content": assistant_response})
```
- Handles user input and appends messages to the chat history.
- Calls `get_ai_response()` to get an AI-generated reply.
- Displays messages interactively.
- Ends the chat when the user types `exit`.

### 5. Connecting UI Elements
```python
send_button.on_click(lambda _: asyncio.create_task(handle_chat(_)))
```
- Triggers message processing when the send button is clicked.

## Ethical Guidelines
The chatbot follows strict guidelines to:
- Avoid harmful or misleading content.
- Prevent copyright infringements.
- Ensure safe, ethical, and respectful interactions.

## How to Run
1. Open the Jupyter Notebook.
2. Run all cells to initialize the interface.
3. Type messages and click **Send** to interact.
4. Type `exit` to end the conversation.

## Future Enhancements
- **Voice Input & Output Support**
- **Persistent Chat History**
- **Enhanced UI with Styling**
- **Integration with Other AI Models**

## License
This project is open-source and available for use under the MIT License.

