
# Basic API for a chatbot using Groqcloud

Created a custom api using Groqcloud on Google collab




## Deployment

To deploy this project run

```bash
!pip install groq
!pip install gradio

```

```bash
from google.colab import userdata
api = userdata.get('api')

import os

from groq import Groq

client = Groq(
    api_key=api,
)

chat_completion = client.chat.completions.create(
    messages=[
        {
            "role": "user",
            "content": "Explain the importance of fast language models",
        }
    ],
    model="llama3-8b-8192",
)

print(chat_completion.choices[0].message.content)

```
## For an interactive session with LLM using gradio

```bash
import os
import ipywidgets as widgets
from IPython.display import display, clear_output

# Assuming you have already installed the required library
# !pip install groq

# Function to send request and display response
def send_request(api_key, user_message, model_name):
    from groq import Groq  # Import inside function to avoid errors if library isn't installed
    
    try:
        # Initialize the client with the API key
        client = Groq(api_key=api_key)
        
        # Send the user message to the Groq API
        chat_completion = client.chat.completions.create(
            messages=[{"role": "user", "content": user_message}],
            model=model_name,
        )
        
        # Display the response
        response_output.clear_output()
        with response_output:
            print(chat_completion.choices[0].message.content)
    
    except Exception as e:
        # Handle errors gracefully
        response_output.clear_output()
        with response_output:
            print(f"Error: {e}")

# Widgets for user input
api_key_input = widgets.Password(description="API Key:")
user_message_input = widgets.Textarea(
    description="Message:", 
    placeholder="Enter your message here", 
    layout=widgets.Layout(width="100%", height="80px")
)
model_name_input = widgets.Text(
    description="Model:", 
    value="llama3-8b-8192", 
    placeholder="Enter model name"
)
submit_button = widgets.Button(description="Send", button_style="success")
response_output = widgets.Output()

# Function to handle button click
def on_submit_button_clicked(b):
    send_request(api_key_input.value, user_message_input.value, model_name_input.value)

# Link button click to function
submit_button.on_click(on_submit_button_clicked)

# Display widgets
display(widgets.VBox([
    api_key_input, 
    user_message_input, 
    model_name_input, 
    submit_button, 
    response_output
]))

```
