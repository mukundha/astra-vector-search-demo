Credits: [Kiyu Gabriel](https://github.com/qzg) for the Original Version

Quick demo to showcase 
- [Vector search capabilities in Astra DB](https://docs.datastax.com/en/astra-serverless/docs/vector-search/overview.html) 
- Build Generative AI applications powered by your own data!

### Usecase

Let's say you are in a e-commerce company. You see all these new advancements in the Large language models and wonder how a ChatGPT like assistant specific for your business would be 1) very cool and 2) super helpful for your customers to help with their purchases.

For eg, when a customer asks

`What equipement would you recommend for a computer workstation setup costing less than $2000?`

You want your assistant / bot / agent generate not a product listing, but a useful response based on your products and user's preference that includes the recommended choice, rationale, alternate options. 

With Vector search in Astra DB (and ofcourse the advancements in the LLM world), this is now super easy, you will learn how in this demo.


### Context

Let's start simple with your product data.

| product_id |	product_name |	description | 	price | 
| -- | -- | -- | -- 
552	| Sony Turntable - PSLX350H | 	Sony Turntable - PSLX350H/ Belt Drive System/ 33-1/3 and 45 RPM Speeds/ Servo Speed Control/ Supplied Moving Magnet Phono Cartridge/ Bonded Diamond Stylus/ Static Balance Tonearm/ Pitch Control 580	Bose Acoustimass 5 Series III Speaker System - AM53BK	Bose Acoustimass 5 Series III Speaker System - AM53BK/ 2 Dual Cube Speakers With Two 2-1/2' Wide-range Drivers In Each Speaker/ Powerful Bass Module With Two 5-1/2' Woofers/ 200 Watts Max Power/ Black Finish |$399.00 
580	| Bose Acoustimass 5 Series III Speaker System |  AM53BK	Bose Acoustimass 5 Series III Speaker System - AM53BK/ 2 Dual Cube Speakers With Two 2-1/2' Wide-range Drivers In Each Speaker/ Powerful Bass Module With Two 5-1/2' Woofers/ 200 Watts Max Power/ Black Finish	| $399.00 | 
.. | .. | .. | .. |

We are using [OpenAI GPT models](https://platform.openai.com/docs/guides/gpt) to build this demo.

If you are able to pass necessary context and prompts to GPT models, it can generate useful response using this context.

Here are some of the sample prompts:

`"You're a chatbot helping customers with questions and helping them with product recommendations"`

`"Please be friendly and talk to me like a person, don't just give me a list of recommendations"`


Now comes the big question, the context - in this case, you need to be able to pass your Product information based on user's input.

#### What you have

1. Product listing, with name, description and price in your database.
2. User's input as a free-form text

#### What you need

1. Understand the intent of user input: You need to know What they meant vs  What they said.

2. Map this intent to a list of Products in your catalog

3. Retrive more details, for eg, description, price about these products from your DB

4. Pass it to GPT, Your users are happy!

1 & 2 were usually hard in the past, not anymore. Embeddings and Vector search to the rescue. 

If you are not familiar with how embeddings work, suggest to read a quick intro about it. Its a very interesting idea. [Add Links].

(Suggest to try out the [Optional] Visualize Embeddings step to explore the embeddings.)

There are several open, free and commercial embedding models and APIs available depending on your usecase. 
This demo shows you how you can use 
1. [Universal Sentence Encoder](https://tfhub.dev/google/universal-sentence-encoder/4) - Free
2. [OpenAI Embeddings](https://platform.openai.com/docs/guides/embeddings) - Commercial

### Solution

1. Use one of the above technique to convert your Product details into embedding vectors and store it in Vector search enabled Astra DB

2. Take User input, use the above technique to convert to a embedding Vector - v1

3. Do a Vector search in Astra DB to find Approximate nearest neighbours (ANN) for v1. This will return Vectors (i.e, Products) that are nearest (probably related to user's intent) to v1.

4. Now use the Product results to pass it as context to GPT and voila!

GPT generated response

```If you're looking for a dishwasher that offers a sleek and modern look, LG has a few great options for you to choose from. The LG LDF6920BB is a fully integrated built-in dishwasher that comes with a sleek black finish, allowing it to easily blend in with your kitchen decor. It features a large 16-place settings capacity, an adjustable upper rack to fit your larger dishware, and a LoDecibel quiet operation that makes it silent when in use.

Another great option would be the LG LDS4821WW, a semi-integrated built-in dishwasher, which still offers a similar look and features to the LDF6920BB, but comes with a white finish instead. Like the LDF6920BB, it also features a large 16-place setting capacity, an adjustable upper rack and a LoDecibel quiet operation for silent use.

If you're looking for a dishwasher with a modern and elegant design, you could consider the LG LDF6920ST, which features a stainless steel finish and a fully integrated electronic control panel. It also has a large 16-place setting capacity, an adjustable upper rack, and a LoDecibel quiet operation, making it a great choice for any modern kitchen.
```

Have fun!

