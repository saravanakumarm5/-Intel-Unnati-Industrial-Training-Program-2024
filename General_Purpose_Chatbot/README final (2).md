
# build_chatbot and finetuning

1. INTRODUCTION:

Overview of the Report:

This report explores the development and fine-tuning of chatbots using two Jupyter notebooks, build_chatbot_on_spr.ipynb and single_node_finetuning_on_spr, on the Intel Developer Cloud. It covers the setup and configuration of the chatbot pipeline, the fine-tuning process, and practical applications of the developed chatbot. The goal is to demonstrate the capabilities of Intel's AI tools in creating efficient and optimized chatbot solutions.

Purpose and Objectives:

The purpose of this report is to demonstrate the creation and fine-tuning of chatbots using Intel Developer Cloud's AI tools. The objectives are to:
1.	Illustrate the chatbot development process.
2.	Explore fine-tuning techniques on a single node.
3.	Evaluate the effectiveness of Intel's AI extensions.
4.	Identify practical applications of the developed chatbot.
5.	Suggest areas for future improvements.
This report aims to provide concise and practical insights into using Intel's AI technologies for chatbot development.

2. INTEL DEVELOPER CLOUD:

Introduction to Intel Developer Cloud:

Intel Developer Cloud offers scalable, high-performance computing resources and advanced AI tools to help developers build, optimize, and deploy AI and machine learning applications efficiently.

Benefits and Features:

Intel Developer Cloud provides scalability, high performance, and access to advanced AI tools, enabling efficient development and deployment of AI and machine learning applications.

3. INTEL EXTENSION FOR TRANSFORMERS:

Overview of Intel Extension for Transformers:

The Intel Extension for Transformers enhances transformer models with optimizations for performance and efficiency, including features like mixed precision training and inference.

Key Features and Capabilities:

The Intel Extension for Transformers enhances performance with optimized algorithms and supports mixed precision, improving efficiency in AI model processing.

4. BUILDING A CHATBOT:

Overview:

Chatbots are AI-powered applications designed to simulate human conversation through text or voice. They play a crucial role in customer service, information retrieval, and automation, enhancing user engagement and operational efficiency.

Pipeline Configuration:

PipelineConfig defines the configuration settings for building and optimizing AI models. MixedPrecisionConfig within it enhances performance by utilizing lower precision arithmetic, optimizing both speed and efficiency in model computations.

Building the Chatbot:

Using build_chatbot in Intel Developer Cloud involves:

Importing Libraries: Include necessary modules.
Configuring Pipeline: Define settings like PipelineConfig.
Building the Chatbot: Use build_chatbot to create and configure the chatbot instance.

Code:

from intel_extension_for_transformers.neural_chat import build_chatbot, PipelineConfig
from intel_extension_for_transformers.transformers import MixedPrecisionConfig
config = PipelineConfig(optimization_config=MixedPrecisionConfig())
chatbot = build_chatbot(config)
response = chatbot.predict(query="what is intel")
print(response)

Chatbot Prediction:

Use the predict method of the chatbot instance.
Example:
response = chatbot.predict(query="example query")
print(response)
This predicts the response for "example query" using the configured chatbot instance.

5. SINGLE-NODE FINETUNING:

Overview:

Fine-tuning optimizes pre-trained models for specific tasks like NLP and chatbots.
Enhances model performance by adapting it to new data or tasks efficiently.

Setting Up the Environment:

Install required libraries and dependencies.
Configure hardware resources for fine-tuning tasks.
Ensure compatibility with Intel's optimized tools for efficient processing.

Fine-tuning Process:

Dataset Preparation: Preparing dataset for training.
Model Configuration: Setting up model architecture and parameters.
Training: Executing the fine-tuning process on a single node.
Evaluation: Assessing the model's performance with validation metrics.

Code:

Finetune the model on Alpaca-format dataset to conduct text generation:

from transformers import TrainingArguments
from intel_extension_for_transformers.neural_chat.config import (
    ModelArguments,
    DataArguments,
    FinetuningArguments,
    TextGenerationFinetuningConfig,
)
from intel_extension_for_transformers.neural_chat.chatbot import finetune_model
model_args = ModelArguments(model_name_or_path="meta-llama/Llama-2-7b-chat-hf")
data_args = DataArguments(train_file="alpaca_data.json", validation_split_percentage=1)
training_args = TrainingArguments(
    output_dir='./tmp',
    do_train=True,
    do_eval=True,
    num_train_epochs=3,
    overwrite_output_dir=True,
    per_device_train_batch_size=4,
    per_device_eval_batch_size=4,
    gradient_accumulation_steps=2,
    save_strategy="no",
    log_level="info",
    save_total_limit=2,
    bf16=True,
)
finetune_args = FinetuningArguments()
finetune_cfg = TextGenerationFinetuningConfig(
            model_args=model_args,
            data_args=data_args,
            training_args=training_args,
            finetune_args=finetune_args,
        )
finetune_model(finetune_cfg)

Finetune the model on the summarization task:

from transformers import TrainingArguments
from intel_extension_for_transformers.neural_chat.config import (
    ModelArguments,
    DataArguments,
    FinetuningArguments,
    TextGenerationFinetuningConfig,
)
from intel_extension_for_transformers.neural_chat.chatbot import finetune_model
model_args = ModelArguments(model_name_or_path="meta-llama/Llama-2-7b-chat-hf")
data_args = DataArguments(dataset_name="cnn_dailymail", dataset_config_name="3.0.0")
training_args = TrainingArguments(
    output_dir='./tmp',
    do_train=True,
    do_eval=True,
    num_train_epochs=3,
    overwrite_output_dir=True,
    per_device_train_batch_size=4,
    per_device_eval_batch_size=4,
    gradient_accumulation_steps=2,
    save_strategy="no",
    log_level="info",
    save_total_limit=2,
    bf16=True
)
finetune_args = FinetuningArguments(task='summarization')
finetune_cfg = TextGenerationFinetuningConfig(
            model_args=model_args,
            data_args=data_args,
            training_args=training_args,
            finetune_args=finetune_args,
        )
finetune_model(finetune_cfg)

Finetune the model on the code generation task:

from transformers import TrainingArguments
from intel_extension_for_transformers.neural_chat.config import (
    ModelArguments,
    DataArguments,
    FinetuningArguments,
    TextGenerationFinetuningConfig,
)
from intel_extension_for_transformers.neural_chat.chatbot import finetune_model
model_args = ModelArguments(model_name_or_path="meta-llama/Llama-2-7b-chat-hf")
data_args = DataArguments(dataset_name="theblackcat102/evol-codealpaca-v1")
training_args = TrainingArguments(
    output_dir='./tmp',
    do_train=True,
    do_eval=True,
    num_train_epochs=3,
    overwrite_output_dir=True,
    per_device_train_batch_size=4,
    per_device_eval_batch_size=4,
    gradient_accumulation_steps=2,
    save_strategy="no",
    log_level="info",
    save_total_limit=2,
    bf16=True
)
finetune_args = FinetuningArguments(task='code-generation')
finetune_cfg = TextGenerationFinetuningConfig(
            model_args=model_args,
            data_args=data_args,
            training_args=training_args,
            finetune_args=finetune_args,
        )
finetune_model(finetune_cfg)


Results and Analysis:

Evaluate using standard metrics like accuracy or F1-score.
Analyze performance based on speed and resource utilization.
Key observations highlight improvements or areas for optimization in model performance.

6. PRACTICAL APPLICATIONS:

Use Cases of the Developed Chatbot:

Real-world Applications: Enhance customer support, automate FAQ handling, and streamline information retrieval across industries like retail, healthcare, and finance.

Potential Integrations and Deployments:

Integration Strategies: Integrate with existing CRM systems, websites, and mobile apps for seamless user interaction.
Deployment: Deploy on cloud platforms or edge devices for scalable and efficient performance.

7. CONCLUSION:

Summary of Key Points:

The report demonstrated the creation and fine-tuning of chatbots using Intel Developer Cloud's AI tools.
Key aspects covered include chatbot development, fine-tuning techniques, and practical applications.

Future Work and Improvements:

Future research could focus on enhancing chatbot capabilities through more complex AI models.
Improvements in scalability and integration with diverse platforms could further enhance usability and performance.
Suggests potential areas for further research and development.



##Documentation
Build chatbot on spr

https://www.canva.com/design/DAGLCWy4YuM/k3LgpATyG17PbN0-n8rPrA/view?utm_content=DAGLCWy4YuM&utm_campaign=designshare&utm_medium=link&utm_source=editor

Single node finetuning
https://www.canva.com/design/DAGLCfnNe40/WQLwHLUa8DoPdvYX0LI5ag/view?utm_content=DAGLCfnNe40&utm_campaign=designshare&utm_medium=link&utm_source=editor
