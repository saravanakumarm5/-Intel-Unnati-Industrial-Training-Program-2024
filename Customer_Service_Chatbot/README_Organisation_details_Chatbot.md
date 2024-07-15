
# Introduction to GenAI and Simple LLM Inference on CPU and finetuning of LLM Model to create a Custom Chatbot

What sets our project apart is the innovative approach to chatbot development. Not only have we created a customer-facing chatbot that provides answers and solutions, but we've also designed a parallel chatbot that gathers organizational information and data from the business itself. This dual-chatbot system enables the customer-facing chatbot to provide highly accurate and personalized responses to customer queries, leveraging the rich data and insights gathered from the organization. This unique approach empowers small businesses to deliver exceptional customer service, setting them apart in a competitive market.


## Data retrieval about the organisation

### 1.Libraries Installation and Imports:

'pip install transformers gradio' installs the necessary libraries.

'gradio' is imported to create the interactive user interface.

### 2.Global Variables and Questions:

responses: A global list to store the user's answers.

questions: A predefined list of questions the chatbot will ask the user.

### 3.Chatbot Response Function:

chatbot_response(user_input, state):
Handles the user's input and determines the next step.

state contains the current list of responses and the current question index.

The function updates the state, decides which buttons should be visible, and clears the user input for the next question.

### 4.Navigation Functions:
previous_question(state): Moves back to the previous question and displays the previous answer.

submit_data(state): Saves all responses to a file and displays a completion message.

save_responses_to_file(data): Writes the responses to a text file.

reset_chatbot(): Resets the chatbot to its initial state.

### 5.Gradio Interface Setup:
#### Layout:

chatbot_output: A textbox to display questions or messages.

user_input: A textbox where the user types their response.

Buttons for "Next", "Previous", "Submit Data", and "Restart".

#### Button Click Functions:

next_button.click(): Moves to the next question.

prev_button.click(): Moves to the previous question.

submit_data_button.click(): Submits and saves the data.

gr.Button("Restart").click(): Resets the chatbot.

#### Initial Load:

demo.load(): Loads the initial state of the chatbot.

### 6.Launching the App:

demo.launch(): Starts the Gradio app.

## Explanation:

### Starting the Chatbot:

When you first open the chatbot, it greets you and asks for the name of your organization. It's designed to help you set up a customized customer service bot by gathering some essential information about your business.

### Answering Questions:

The chatbot will ask you a series of questions one by one. You just type your response in the provided textbox and click "Next" to move to the next question.

### Navigating Through Questions:

If you make a mistake or want to change an answer, you can click "Previous" to go back to the last question and update your response.
You can keep moving back and forth between questions using the "Next" and "Previous" buttons until you're satisfied with all your answers.

### Submitting Your Responses:

Once you reach the last question and provide your answer, a "Submit Data" button will appear. Clicking this button will save all your responses to a file and show a message thanking you for completing the questionnaire.

### Restarting the Process:

If you need to start over for any reason, you can click the "Restart" button to reset the chatbot to its initial state and begin answering the questions again.

### Completion:

After submission, the chatbot saves your responses, indicating that the customization process for your customer service bot is complete.
## Gradio Interface to Read and Display Responses File Content

### Function to Read File:

read_file(): This function attempts to open and read the content of responses.txt.

If the file is found, it reads the content and counts the number of lines by counting the newline characters (\n) and adding 1.

If the file is not found, it returns an error message and sets the line count to 0.
Gradio Interface Setup:

### Creating the Interface:

The interface is defined within a gr.Blocks() context, which allows for more complex layouts and interactions.

A column layout is used to organize the elements.

#### Two gr.Textbox elements are created:
file_content: A non-interactive textbox with 20 lines to display the content of the file.

line_count: A non-interactive textbox with 1 line to display the number of lines in the file.

### Loading the File Content:

demo.load(): This function is called when the app starts. It runs the read_file function and sets the outputs to the file_content and line_count textboxes.

### Launching the App:

demo.launch(): This starts the Gradio app, making it available to interact with.
## PDF Upload and Text Extraction Interface

### Function to Convert PDF to Text:

pdf_to_text(pdf_file):
Uses pdfminer.high_level.extract_text to extract text from the uploaded PDF file.

Counts the number of lines in the extracted text by splitting it into lines and calculating the length.

Returns the extracted text and the line count.

In case of an error, returns the error message and a line count of 0.

### Function to Handle File Upload and Conversion:

handle_file_upload(pdf_file):
Checks if a file has been uploaded.

If not, returns a message asking the user to upload a PDF file.

Calls pdf_to_text to convert the uploaded PDF to text and count the lines.

Saves the extracted text to a file named pdf_text.txt.

Reads the content of pdf_text.txt to display it.
Prepares a response message with the PDF file name, number of lines, and the saved text file name.

Returns the response message, text file name, text content, and a success flag.

In case of an error, returns an error message and sets the success flag to False.

### Gradio Interface Setup:

#### Title and Description:

title: "PDF UPLOAD"

description: Provides context for the user, explaining the need to upload their license terms, conditions policies, and any other related information to finalize the questionnaire.

#### Creating the Interface:

gr.Interface:
fn=handle_file_upload: The function to handle the file upload and conversion.

inputs="file": Specifies that the input will be a file upload.

outputs=["text", "text", "text"]: Specifies three text outputs:
A response message.
The name of the text file where the extracted text is saved.
The content of the extracted text.

title and description: Displayed at the top of the interface to guide the user.

### Launching the App:

demo.launch(): Starts the Gradio app, making it available for interaction.

This Gradio interface enables users to upload a PDF file, converts the PDF content to text, counts the number of lines, saves the text to a file, and displays the results. It provides a convenient way to process and view the content of a PDF file, with clear instructions and a user-friendly interface.
## File Content Appender

### Function Definition:

append_file(file1_path, file2_path): This function takes two file paths as input:

file1_path: The path of the first file, to which content will be appended.

file2_path: The path of the second file, whose content will be appended to the first file.

### Appending File Content:

#### File Handling:

Opens file2_path in read mode ('r') and file1_path in append mode ('a'), both with UTF-8 encoding to handle text properly.

Adds a newline ("\n") before appending the content of file2_path to ensure the appended text starts on a new line (optional but useful for readability).

Reads the content of file2_path and writes it to file1_path.

#### Error Handling:

FileNotFoundError: Catches errors if the specified files are not found, returning a descriptive error message.

Generic Exception Handling: Catches any other unexpected exceptions and returns a generic error message.

### Example Usage:

file1_path: Path to responses.txt, the file to which content will be appended.

file2_path: Path to pdf_text.txt, the file whose content will be appended.

Result: Calls append_file with the specified paths and prints the result message, indicating success or the nature of any error encountered.
## Robust File Reader

### Function Definition:

read_file(file_path): Takes the path to a text file as input (file_path).

### Reading the File:

#### File Handling:
Opens the file specified by file_path in read mode ('r') with UTF-8 encoding.

Uses the errors='replace' parameter to replace any problematic characters that cannot be decoded with the default replacement character.

Reads the entire content of the file into a string (content).

#### Error Handling:

FileNotFoundError: Catches the specific error if the file is not found, returning a descriptive error message.

Generic Exception Handling: Catches any other unexpected exceptions and returns a generic error message indicating an unexpected error occurred.

### Example Usage:

Calls read_file with the path to responses.txt and prints the content or error message.


## Demo

Link to demo that illustrates how the Organisation_details_Chatbot.ipynb works

https://www.canva.com/design/DAGK8Na3Z-A/1g3sswgJ7tc2WuCJ1PD_uw/watch?utm_content=DAGK8Na3Z-A&utm_campaign=designshare&utm_medium=link&utm_source=editor