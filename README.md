## Development and Deployment of a 'Chat with LLM' Application Using the Gradio Blocks Framework

### AIM:
To design and deploy a "Chat with LLM" application by leveraging the Gradio Blocks UI framework to create an interactive interface for seamless user interaction with a large language model.

### PROBLEM STATEMENT:
The objective is to develop an intuitive and interactive interface that enables users to communicate with a large language model (LLM) effectively. The application should handle user inputs, generate responses from the LLM, and display the results in real-time.


### DESIGN STEPS:

TEP 1:Set Up the Environment
Install the necessary libraries, such as gradio and transformers.
Choose an appropriate LLM (e.g., OpenAI's GPT, Hugging Face's GPT-2/3.5/4).
Ensure GPU support for faster inference, if required.
STEP 2:Create the Gradio Blocks Interface
Use gr.Blocks to design a modular and interactive layout.
Define the components such as Textbox for user input, Chatbot for displaying messages, and Button for interaction.
STEP 3: Integrate the LLM with the Interface
Test the application locally to ensure smooth functionality.
Deploy the app using platforms like Hugging Face Spaces, Streamlit Cloud, or a custom server.

### PROGRAM:
```
import os
import io
import IPython.display
from PIL import Image
import base64 
import requests 
requests.adapters.DEFAULT_TIMEOUT = 60

from dotenv import load_dotenv, find_dotenv
_ = load_dotenv(find_dotenv()) # read local .env file
hf_api_key = os.environ['HF_API_KEY']

# Helper function
import requests, json
from text_generation import Client

#FalcomLM-instruct endpoint on the text_generation library
client = Client(os.environ['HF_API_FALCOM_BASE'], headers={"Authorization": f"Basic {hf_api_key}"}, timeout=120)

def format_chat_prompt(message, chat_history):
    prompt = ""
    for turn in chat_history:
        user_message, bot_message = turn
        prompt = f"{prompt}\nUser: {user_message}\nAssistant: {bot_message}"
    prompt = f"{prompt}\nUser: {message}\nAssistant:"
    return prompt

def respond(message, chat_history):
        formatted_prompt = format_chat_prompt(message, chat_history)
        bot_message = client.generate(formatted_prompt,
                                     max_new_tokens=1024,
                                     stop_sequences=["\nUser:", "<|endoftext|>"]).generated_text
        chat_history.append((message, bot_message))
        return "", chat_history

with gr.Blocks() as demo:
    chatbot = gr.Chatbot(height=240) #just to fit the notebook
    msg = gr.Textbox(label="Prompt")
    btn = gr.Button("Submit")
    clear = gr.ClearButton(components=[msg, chatbot], value="Clear console")

    btn.click(respond, inputs=[msg, chatbot], outputs=[msg, chatbot])
    msg.submit(respond, inputs=[msg, chatbot], outputs=[msg, chatbot]) #Press enter to submit

gr.close_all()
demo.launch(share=True, server_port=int(os.environ['PORT3']))
```
### OUTPUT:
<img width="1447" height="708" alt="image" src="https://github.com/user-attachments/assets/a6b9de67-6f60-49c2-81ca-9d05425fd148" />




### RESULT:
Hence to design and deploy a "Chat with LLM" application by leveraging the Gradio Blocks UI framework to create an interactive interface for seamless user interaction with a large language model is verified.
