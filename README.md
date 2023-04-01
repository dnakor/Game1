import openai
import tkinter as tk
import os

# Set the API key
openai.api_key = os.environ["OPENAI_API_KEY"]

# Function to get the response from the AI
def get_response(prompt, engine):
    completions = openai.Completion.create(
        engine=engine,
        prompt=prompt,
        max_tokens=150,
        n=1,
        stop=None,
        temperature=0.7,
    )

    message = completions.choices[0]["text"]
    return message.strip()

# Main application class
class Application(tk.Frame):
    def __init__(self, master=None):
        super().__init__(master)
        self.master = master
        self.master.title("AI Chatbot")
        self.master.geometry("800x600")
        self.pack(fill=tk.BOTH, expand=True)
        self.create_widgets()

    # Function to create the UI widgets
    def create_widgets(self):
        # Create the chat frame
        self.chat_frame = tk.Frame(self, padx=15, pady=15, bg="#282C34")
        self.chat_frame.pack(fill=tk.BOTH, expand=True)

        # Create the prompt label
        self.prompt_label = tk.Label(self.chat_frame, text="Enter your prompt:", bg="#282C34", fg="#ABB2BF", font=("Helvetica", 12))
        self.prompt_label.pack(pady=5)

        # Create the prompt entry
        self.prompt_entry = tk.Entry(self.chat_frame, font=("Helvetica", 12))
        self.prompt_entry.pack(fill=tk.X, pady=5)

        # Create the submit button
        self.submit_button = tk.Button(self.chat_frame, text="Submit", command=self.submit_prompt, font=("Helvetica", 12))
        self.submit_button.pack(pady=5)

        # Create the response label
        self.response_label = tk.Label(self.chat_frame, text="Response:", bg="#282C34", fg="#ABB2BF", font=("Helvetica", 12))
        self.response_label.pack(pady=5)

        # Create the response text box
        self.response_text = tk.Text(self.chat_frame, height=10, wrap=tk.WORD, bg="#3C3F58", fg="#ABB2BF", font=("Helvetica", 12), width=80)
        self.response_text.pack(fill=tk.BOTH, expand=True, pady=5)

        # Create the command prompt button
        self.cmd_button = tk.Button(self, text="Open Command Prompt", command=self.open_command_prompt, font=("Helvetica", 12))
        self.cmd_button.pack(side=tk.BOTTOM, pady=10)

    # Function to handle submitting a prompt
    def submit_prompt(self):
        # Get the user's input
        prompt = self.prompt_entry.get()
        if not prompt.strip():
            # If the input is empty, show an error message
            self.response_text.delete("1.0", tk.END)
            self.response_text.insert(tk.END, "Please enter a valid prompt.")
            return

        # Get the AI's response
        response = get_response(prompt, "gpt-3.5-turbo")
        # Update the response text box with the AI's response
        self.response_text.delete("1.0", tk.END)
        self.response_text.insert(tk.END, response)

    # Function to open the command prompt
    def open_command_prompt(self):
        os.system("start cmd")

# Create the main window and run the application
root = tk.Tk()
app = Application(master=root)
app.mainloop()

