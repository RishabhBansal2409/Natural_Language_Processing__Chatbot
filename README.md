# Machine Learning (ML) Powerered Chatbot

We all might have experienced talking to a virtual assistant/Chatbot. It is widely used in various industries such as **Banking , Telecom , Transportation** etc. **Chatbot is used** in various senses but it's core function is **to address the customer grievances thereby effectively catering to the various needs of the customer** in a timely manner.

Chatbot can be broadly classified into two categories :

a) `Rule Based` - Bots answer questions based on some pre-defined rules.These bots are inexpensive , easy to train and can handle simple queries but the downside is they fail to manage the complex ones.

b) `Self Learning` - Bots use some Machine Learning-based approaches which makes them more efficient than the rule-based bots.

Self Learning Bots can be further divided into two categories `Retrieval Based` or `Generative based`. 

Primary difference between these bots is that **Retrieval based bots selects a response from a library of pre-defined responses** whereas **Generative bots** , as the name implies **is capable of generating the answers on it's own**. 

Let's go deep dive and learn how retrieval based bots answer a query by an user.

Architecture :


`User Input`

First thing a user encounters is an interface where **s/he will input the query**.In the bot I have built , I have made use of the Telegram channel as an interface where the user can give the input in the form of a message.

`Classifying Intent of the message using ML model`

Primary thing needed to build a ML model is a Dataset. Here **Dataset** consists of nearly half a million rows and **contains two columns , text and tag.Text column contains set of sentences and tag will be either Dialogue or a Stack Overflow**.The text column , being the independent variable is **pre-processed (removal of most common words like a , the etc) and is converted into a matrix of numbers(TF-IDF matrix) using a vectorizer**.We can now split the dataset into training and testing and can train the model using the training datset.Testing dataset is used to check how the model works on unseen data.**Logistic Regression model works great in my case and can now be used to predict the intent (Dialogue vs Stack Overflow) of the query by the user**.

`When the Intent identified is Stack Overflow`

Two things that the bot needs to do when the user asks a Stack Overflow(SO) query are :-

**1. Identify which programming language/tag the query belongs to ?**
**2. Find the most similar title's ID to the query in the identified tag space ?**

To address the first question , **an ML multi-class classifier model works in the backend** , which is trained on set of stack overflow titles (independent variable) and the programming language/tag (dependent variable) associated with each title.Same preceprocessing and conversions steps are applied on the title column before being supplied to the model.**This learned model (Logistic Regression again) predicts the tag associated with the query given by the user.**

To answer the second question , we need to **convert each title into embeddings (also known as word vectors). Embeddings are representation of words in a high dimensional space**.Title is first broken into set of words and for each word , an embedding is assigned by a pre trained model (used google news' embeddings which represent each words in 300 dimnesions).To know a title's embedding , each words's embeddings are averaged over all the 300 dimensions.Similary , query's embedding is evaluated. **Titles' embeddings (belonging to the identified tag) are compared with the query's embedding. The embedding which is at the least distance with the query's embedding is selected and the corresponding Stack overflow link is sent by the bot as a reply.**

`When the Intent identified is dialogue` 

In order to reply to the user's simple dialogue such as 'What are you doing' , 'Who is Spiderman' , the chatbot leverages chatterbot library in python which trains the chatbot on the english corpus . This library helps in generating automated responses to a user’s chtchat query by using a selection of ML algorithms.






the message either into a Simple Dialogue or Stack Overflow and answers appropriately using pre trained machine learning model
