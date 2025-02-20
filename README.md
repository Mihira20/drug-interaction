Drug Interaction RAG (Retrieval-Augmented Generation) system

This project implements a sophisticated drug interaction information retrieval system using advanced natural language processing techniques. The system combines a fine-tuned BERT model with a TF-IDF based retrieval mechanism to provide accurate and relevant information about drug interactions.


Key components of the project include:

Data Preprocessing: The project starts by cleaning and structuring a comprehensive drug interaction dataset, 
                    handling tasks such as removing duplicates, normalizing text, and creating meaningful interaction descriptions.

Retrieval Component: A TF-IDF vectorizer is used to create a searchable index of drug interactions. 
                     This allows for efficient retrieval of relevant passages based on user queries.

BERT Model Fine-tuning: A BERT model is fine-tuned on the processed drug interaction data to classify interaction types. 
                        This adds a powerful understanding of drug interaction contexts to the system.

RAG Pipeline: The heart of the system is a RAG pipeline that combines the retrieval component with the fine-tuned BERT model. 
              It retrieves relevant passages for a given query and then uses the BERT model to generate an informative response.

Interactive Query Interface: The system provides an interactive command-line interface where users can input queries about drug interactions and receive detailed responses.


This project demonstrates the application of modern NLP techniques to the critical field of pharmacology, 
potentially aiding researchers and healthcare professionals in quickly accessing relevant drug interaction information.


  A) Data Preprocessing:
  The data preprocessing stage begins with loading a drug interaction dataset from a gzip-compressed TSV file. 
  Initially, the dataset contains information about drug-target interactions, including details such as drug names, target proteins, activity values, and interaction types.

  The initial dataset has 19,378 entries and 20 columns, including 'DRUG_NAME', 'TARGET_NAME', 'TARGET_CLASS', 'ACT_VALUE', 'ACT_TYPE', 'ACTION_TYPE', and 'ORGANISM'. 
  Numerical columns like 'STRUCT_ID' and 'ACT_VALUE' show a wide range of values, with 'ACT_VALUE' ranging from 1.2 to 13.0.

  The preprocessing steps included:

  --Removing duplicate entries, reducing the dataset to 19,149 rows.

  --Converting text in 'DRUG_NAME', 'TARGET_NAME', and 'TARGET_CLASS' columns to lowercase for consistency.

  --Removing leading and trailing whitespace from all string columns.

  --Creating a unique 'interaction_id' for each entry.

  --Generating an 'interaction_text' field by combining information from 'DRUG_NAME', 'TARGET_NAME', 'TARGET_CLASS', and 'ACTION_TYPE'.

  The final preprocessed dataset, saved as 'processed_drug_interactions.csv', contains six columns: 
  'interaction_id', 'DRUG_NAME', 'TARGET_NAME', 'TARGET_CLASS', 'ACTION_TYPE', and 'interaction_text'. 
  This structured format provides a clean, consistent basis for further analysis and model training, with each row representing a 
  unique drug-target interaction described in a standardized text format.

  
  B) Retrieval Component:
  The Retrieval Component in this drug interaction RAG system uses TF-IDF (Term Frequency-Inverse Document Frequency) 
  vectorization to create an efficient searchable index of drug interactions. 
  This technique allows for quick and relevant retrieval of passages based on user queries.

  TF-IDF vectorization works by assigning weights to words in each document (in this case, drug interaction descriptions) 
  based on their frequency within the document and their rarity across all documents. 
  Words that appear frequently in a specific document but are uncommon across the entire dataset receive higher weights, 
  making them more important for that document's representation.

  In the code, the TfidfVectorizer from scikit-learn is used to transform the 'interaction_text' 
  column of the dataset into a TF-IDF matrix. This matrix represents each drug interaction as a vector of weighted terms. 
  When a user submits a query, it's also transformed into a TF-IDF vector, and cosine similarity is used to find the most relevant passages from the dataset.

  This approach allows for fast and memory-efficient searching, as it can quickly compare the query vector 
  to all document vectors in the dataset. It's particularly effective for finding relevant drug interactions based on textual similarity, 
  making it a crucial component of the RAG system's ability to provide accurate and context-relevant responses to user queries.

  
  C) BERT Model Fine-tuning:
  The BERT model fine-tuning step is a crucial part of this drug interaction RAG system. 
  It enhances the system's ability to understand and classify drug interaction types based on the processed data.

  In this process, a pre-trained BERT (Bidirectional Encoder Representations from Transformers) 
  model is adapted to the specific task of classifying drug interaction types. 
  The code uses the 'bert-base-uncased' model as a starting point, which is then fine-tuned on the processed drug interaction dataset.

  The fine-tuning process involves:

  --Preparing the dataset by tokenizing the interaction texts and encoding the action types as labels.

  --Splitting the data into training and validation sets.

  --Setting up a BERT model for sequence classification, with the number of output labels matching the unique action types in the dataset.

  --Training the model using the Hugging Face Trainer API, with specified hyperparameters like learning rate, batch size, and number of epochs.

  This fine-tuned BERT model can then classify new drug interaction descriptions into appropriate action types, 
  providing a deeper understanding of the interaction contexts. This capability is integrated into the RAG system to 
  generate more accurate and context-aware responses to user queries about drug interactions.

  
  D) RAG Pipeline:
  The RAG (Retrieval-Augmented Generation) pipeline is the core of this drug interaction information system. 
  It combines two key components: the retrieval mechanism and the fine-tuned BERT model for generation.

  The pipeline works as follows:

  Retrieval: When a user submits a query, the system first uses TF-IDF vectorization to find relevant passages from the drug interaction dataset. The 
  'retrieve_relevant_passages' function transforms the query into a TF-IDF vector and uses cosine similarity to identify the top 5 most relevant passages.

  Generation: The 'generate_response' function then takes these relevant passages along with the original query and combines them into a single input for the BERT model. 
  This combined input is tokenized and passed through the fine-tuned BERT model, which predicts the most likely action type for the drug interaction.

  Response Construction: The system constructs a detailed response that includes the predicted action type and the relevant passages retrieved. 
  This provides context and supporting information for the prediction.

  User Interface: The 'process_user_query' function creates an interactive interface where users can input queries and receive responses until they choose to quit.

  This RAG approach allows the system to leverage both the broad knowledge captured in the BERT model and the specific information in the drug interaction dataset, 
  providing informative and context-aware responses to user queries about drug interactions.


  E) Interactive Query Interface:
  The interactive query interface is the user-facing component of the drug interaction RAG system. 
  It provides a command-line interface where users can directly interact with the system to get 
  information about drug interactions. 
  
  Here's how it works:

  The interface is implemented in the process_user_query() function, which runs in a loop until the user decides to quit.

  When started, the system displays a welcome message and instructions for use.

  Users can type in natural language queries about drug interactions. For example, they might ask "What is the interaction between aspirin and warfarin?"

  The system processes each query through the RAG pipeline, which retrieves relevant passages and generates a response using the fine-tuned BERT model.

  The response, including the predicted action type and relevant passages, is then displayed to the user.

  This process continues, allowing users to ask multiple questions in a session, until they type 'quit' to exit.

  This interface makes the complex backend of the RAG system accessible to users who may not have technical expertise, 
  allowing them to quickly obtain information about drug interactions in a conversational manner.
  


  

  




