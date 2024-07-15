
# Introduction to GenAI and Simple LLM Inference on CPU and finetuning of LLM Model to create a Custom Chatbot

What sets our project apart is the innovative approach to chatbot development. Not only have we created a customer-facing chatbot that provides answers and solutions, but we've also designed a parallel chatbot that gathers organizational information and data from the business itself. This dual-chatbot system enables the customer-facing chatbot to provide highly accurate and personalized responses to customer queries, leveraging the rich data and insights gathered from the organization. This unique approach empowers small businesses to deliver exceptional customer service, setting them apart in a competitive market.


## AI-Powered Customer Service Chatbot with Contextual Answering

This code sets up an AI-powered chatbot using a pre-trained BERT model for question answering. The chatbot reads context from a text file and answers user queries based on that context. If it cannot find an answer, it provides customer care contact numbers extracted from the text. Here’s a detailed explanation:

1. **Installation and Imports**:
   - **Installations**: The required libraries `transformers` and `torch` are installed.
   - **Imports**: The necessary modules from `transformers`, `torch`, and `re` (for regular expressions) are imported.

2. **Model and Tokenizer Setup**:
   - **Model Name**: Uses `"bert-large-uncased-whole-word-masking-finetuned-squad"`, a BERT model fine-tuned on the SQuAD dataset for question answering.
   - **Loading**: The model and tokenizer are loaded using `AutoTokenizer` and `AutoModelForQuestionAnswering`.

3. **Read Text File**:
   - **Function `read_text_file(file_path)`**: Reads and returns the content of a text file with specified encoding (`latin-1`).

4. **Extract Customer Care Numbers**:
   - **Function `extract_customer_care_numbers(context)`**: Uses a regular expression to find customer care numbers in the context. It returns a list of tuples with names and numbers.

5. **Generate Response**:
   - **Function `generate_response(context, question)`**:
     - Encodes the question and context using the tokenizer.
     - Truncates the context to fit within the model’s maximum length.
     - Uses the model to predict the start and end positions of the answer in the context.
     - Extracts and decodes the answer tokens.
     - If no answer is found, it extracts customer care numbers from the context and returns them.

6. **Load Text File Content**:
   - Reads the content of `responses.txt` and stores it in the `context` variable.

7. **Chatbot Loop**:
   - **Function `chatbot()`**:
     - Runs an interactive loop where the user can ask questions.
     - Calls `generate_response` to get the answer based on the context.
     - Provides an option to exit the loop by typing 'exit'.

8. **Run the Chatbot**:
   - The `chatbot()` function is called to start the chatbot.

### Summary

This script sets up a chatbot that leverages a pre-trained BERT model for question answering. It reads a text file to gather context, answers user queries based on that context, and provides customer care numbers if the answer is not found. The chatbot interacts with users in a loop, offering an engaging and informative experience.
## Demo

Link to demo that illustrates how the Customer_Service_Chatbot.ipynb works

https://www.canva.com/design/DAGK8QJmxQg/ftQbSp2LHp7z_X56GNvooQ/watch?utm_content=DAGK8QJmxQg&utm_campaign=designshare&utm_medium=link&utm_source=editor