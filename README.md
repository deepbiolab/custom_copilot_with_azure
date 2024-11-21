# Custom Copilot with Azure

### Project Overview

These project instructions will guide you through creating and deploying a custom AI copilot using Azure AI Studio. Follow the steps below carefully, and be sure to complete each deliverable for submission.

In this project, you will:

1. Create an AI Studio project and hub.
2. Deploy an AI model and upload product data for indexing.
3. Build and test a custom copilot app using Prompt Flow.
4. Evaluate the app with both automated and manual prompt evaluation.
5. Deploy and test the copilot application.

### Step-by-Step Guide

#### 1. Create an AI Studio Hub and Project

- Log in to [Azure AI Studio](https://ai.azure.com/).

  ![image-20241121113508200](assets/image-20241121113508200.png)

- Click **Create Project**, name your project (`outlander-ai-project`), and create a new hub (`outlander-ai-hub`).

  <img src="assets/image-20241121114026889.png" alt="image-20241121114026889"  />

- When we first time create this project and not create hub yet, after click **Customize**, we will customize below settings. For  key settings in below you can custom by the instruction.

  ![image-20241121114734878](assets/image-20241121114734878.png)

- Leave default configurations unless necessary adjustments are needed(below is my settings), and click **Next** in below.

  ![image-20241121120802182](assets/image-20241121120802182.png)



- Ensure you have an **Azure AI Search** service for indexing data. And finally review the information and click **Create**

  ![image-20241121114944714](assets/image-20241121114944714.png)

  ![image-20241121115535949](assets/image-20241121115535949.png)

#### 2. Deploy an AI Model

- With your project selected, go to **Models + endpoints**.

  ![image-20241121121410538](assets/image-20241121121410538.png)

- Click **+Deploy Model**, choose a base model like `gpt-4o`, and confirm the deployment.

  ![image-20241121121625966](assets/image-20241121121625966.png)

  ![image-20241121121722867](assets/image-20241121121722867.png)

  ![image-20241121121906334](assets/image-20241121121906334.png)

  ![image-20241121122431637](assets/image-20241121122431637.png)

  ![image-20241121122651808](assets/image-20241121122651808.png)

#### 3. Upload Product Data to Azure AI Studio

- Download and unzip the `product-info.zip` from [this GitHub link](https://github.com/Azure-Samples/rag-data-openai-python-promptflow/tree/main/tutorial/data).

- In AI Studio, select the **Data + indexes** blade and click **+New Data**.

  ![image-20241121122847180](assets/image-20241121122847180.png)

- Upload the unzipped product data files or connect to your storage account if preferred.

  ![image-20241121123005228](assets/image-20241121123005228.png)

  ![image-20241121123215337](assets/image-20241121123215337.png)

  ![image-20241121123325596](assets/image-20241121123325596.png)

- After upload files, you can see the files structure in **Data + indexes** panel.

  ![image-20241121123456693](assets/image-20241121123456693.png)

#### 4. Create an AI Search Index

- Create a Embedding model, we can transform the document (uploaded above) to dense embedding which we can use it in RAG system later. Like procedure created for `gpt-4o`, we choose `Models + endpoints` >>> `Deploy model` >>> `Deploy base model` in sequentially, and choose `text-embedding-ada-002` model to create.

  ![image-20241121124142845](assets/image-20241121124142845.png)

- Still you can custom below settings, but i leave here by default and click **Deploy**

  ![image-20241121124343536](assets/image-20241121124343536.png)

- After deployed, we can see the model in the service we created before.

  ![image-20241121124541818](assets/image-20241121124541818.png)

- Go to the **Data + indexes** blade and click **+New Index** after **Indexes**

  ![image-20241121124755977](assets/image-20241121124755977.png)

  ![image-20241121124924496](assets/image-20241121124924496.png)

- Select **Data in Azure AI Studio** as the data source, choose the uploaded data, and proceed.

  ![image-20241121125015098](assets/image-20241121125015098.png)

  ![image-20241121125303308](assets/image-20241121125303308.png)

  ![image-20241121125542058](assets/image-20241121125542058.png)

- Choose your deployed model for text embeddings and create the index.

  ![image-20241121125757914](assets/image-20241121125757914.png)

  ![image-20241121125851637](assets/image-20241121125851637.png)

  ![image-20241121133104319](assets/image-20241121133104319.png)

  

#### 5. Build the Copilot App

- Navigate to the **Chat** blade under Project Playground.

  ![image-20241121133250989](assets/image-20241121133250989.png)

- Select your deployed model and add your data by choosing the created index.

  ![image-20241121133509986](assets/image-20241121133509986.png)

  ![image-20241121133644108](assets/image-20241121133644108.png)

- Click **Prompt Flow**, name your flow (e.g., `Outlander AI Copilot`), and open it.

  ![image-20241121133858092](assets/image-20241121133858092.png)

  ![image-20241121133930169](assets/image-20241121133930169.png)

- Review and understand the default components, such as data retrieval and response generation.

  ![image-20241121135138303](assets/image-20241121135138303.png)

#### 6. Test the Copilot

- Start a compute session by clicking **Start compute session**.

  ![image-20241121140303726](assets/image-20241121140303726.png)

- Test the copilot by entering sample questions such as:

  > Below questions basically stick with the documents we uploaded, so ideally it will reply the relevant response based the document that is purpose of RAG.

  - `How much do the TrailWalker Hiking Shoes cost?`

    ![test01](./assets/outlander_ai_copilot_test01.png)

  - `Which tent is the most waterproof?`

    ![test02](./assets/outlander_ai_copilot_test02.png)

  - `Can the warranty for TrailBlaze pants be transferred?`

    ![test03](./assets/outlander_ai_copilot_test03.png)

- After test, click **Save** for this copilot.

#### 7. Perform Automated Prompt Evaluation

- Create a JSONL or CSV file with evaluation questions and answers (see below for format).
  > [Sample Evaluation Data (JSONL format)](./data/qa_evaluation_data.jsonl)
  >
  > ```
  > {"chat_input": "Which tent is the most waterproof?", 
  > "truth": "The Alpine Explorer Tent has the highest rainfly waterproof rating at 3000m", 
  > "chat_history": []}
  > {"chat_input": "How much do the TrailWalker Hiking Shoes cost?", 
  > "truth": "The TrailWalker Hiking Shoes are priced at $110", 
  > "chat_history": []}
  > ```
  >
  > [Sample Evaluation Data (CSV format)](./data/qa_evaluation_data.csv)
  >
  > | chat_input                                      | truth                                                        | chat_history |
  > | ----------------------------------------------- | ------------------------------------------------------------ | ------------ |
  > | Which  tent is the most waterproof?             | The Alpine Explorer Tent has the  highest rainfly waterproof rating at 3000m | []           |
  > | How  much do the TrailWalker Hiking Shoes cost? | The TrailWalker Hiking Shoes are  priced at $110             | []           |


- In **Prompt Flow**, click **Evaluate**, choose **Automated evaluation**, and then and use your dataset.

  ![image-20241121142553882](../../../../Projects/deepbiolab.github.io/assets/img/image-20241121142553882.png)

  ![image-20241121142724140](assets/image-20241121142724140.png)

  ![image-20241121143218630](assets/image-20241121143218630.png)

- Map inputs and outputs and **Submit** on the next page and then run the evaluation.

  ![image-20241121143617316](assets/image-20241121143617316.png)

  ![image-20241121144300055](assets/image-20241121144300055.png)

#### 8. Manual Prompt Evaluation

- Go to **Evaluation** in AI Studio and Create a **Manual evaluations**

  ![image-20241121144618653](assets/image-20241121144618653.png)

- Add questions and expected responses and run evaluations, right here I only add one.

  - Input: `How much do the TrailWalker Hiking Shoes cost?`
  - Expected response: `The TrailWalker Hiking Shoes are priced at $110.`

  ![image-20241121144810236](assets/image-20241121144810236.png)

-  After run, you can provide feedback using thumbs up or down for the response.
  ![image-20241121145131253](assets/image-20241121145131253.png)

#### 9. Deploy the Copilot

- In **Prompt Flow**, click **Deploy** and name the deployment.

  ![image-20241121145320570](assets/image-20241121145320570.png)

  ![image-20241121145504522](assets/image-20241121145504522.png)

- Verify the deployment details and create, finally you will see the created endpoint in below image.

  ![image-20241121150106569](assets/image-20241121150106569.png)

- Finally, click the created endpoint `outlander-ai-project-znsmp-1` and you will see ready-to-use deployment info

  ![image-20241121151529198](assets/image-20241121151529198.png)

- And we can test some question.

  ![image-20241121152034536](assets/image-20241121152034536.png)

🎉Good luck, and enjoy building your custom Azure AI copilot✨!

