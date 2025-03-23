
# Mosaic AI Agent Framework: Genie を使用してマルチエージェントシステムを作成およびデプロイ

Mosaic AI Agent Framework と [LangGraph](https://blog.langchain.dev/langgraph-multi-agent-workflows/) を使用してマルチエージェントシステムを構築します。ここで、[Genie](https://www.databricks.com/product/ai-bi/genie) はエージェントの 1 つです。
このノートブックでは、以下の操作を行います。
1. LangGraph を使用してマルチエージェントシステムを作成します。
1. Databricks の機能との互換性を確保するために、LangGraph エージェントを MLflow の `ChatAgent` でラップします。
1. マルチエージェントシステムの出力を手動でテストします。
1. マルチエージェントシステムのログを取得し、デプロイします。

この例は、[LangGraph ドキュメント - マルチエージェント スーパーバイザーの例](https://github.com/langchain-ai/langgraph/blob/main/docs/docs/tutorials/multi_agent/agent_supervisor.ipynb)を基にしています。

## Genie エージェントを使用する理由

マルチエージェントシステムは、それぞれに専門能力を持つ複数のAIエージェントで構成されています。Genieは、そのエージェントの1つであり、ユーザーが自然言語を使用して構造化データとやりとりすることを可能にします。

あらかじめ定義されたクエリのみを実行できるSQL関数とは異なり、Genieはユーザーの質問に答えるために新しいクエリを作成できる柔軟性があります。

## 前提条件

- このノートブック内のすべての「TODO」を処理する。
- Genie Spaceを作成します。Databricksのドキュメントを参照してください（[AWS](https://docs.databricks.com/aws/genie/set-up) | [Azure](https://learn.microsoft.com/azure/databricks/genie/set-up))。

#### TODO箇所

- GENIE_SPACE_ID:  事前に作成したGenie Space ID を入力します。
- LLM_ENDPOINT_NAME: Supervisorとして動作するLLMの Endpoint名を入力します。こちらはモデルサービングのエンドポイント名です。LLMはGPT-4o or GPT o1がおすすめ。
- 適切な権限をもったPAT のSecret情報。[Secretの作成方法](https://qiita.com/maroon-db/items/6e2d86919a827bd61a9b)
  - PATに必要な権限
    - Genie Spaceに「CAN RUN」権限でプロビジョニング
    - Genie Spaceを動かすSQL Warehouseに「CAN USE」権限でプロビジョニング
    - Unity Catalogのテーブルに「SELECT」権限でプロビジョニング
    - 基盤となるUnityカタログ関数に対するEXECUTEの提供
- 作成したモデルを保存する Unity Catalog情報を入力。（Catalog / Schema / Model名) 

![Image](https://github.com/maroon-spec/Multiagent_Genie_Demo/blob/main/multiagent-genie-demo.gif)


## 使用方法

1. この Repogitory をDatabricks上の Reposに登録。もしくはノートブック (multiagent_genie_demo.ipyb) をImportする。
2. 前提条件を満たす
3. ノートブックを実行すると、最後にMLflowモデルがUnity Catalog上に保存され、そのモデルを使ったモデルサービングエンドポイントが作成されます。
4. PlayGroundで、作成したエンドポイントを選択すると利用できます。


## 参考サイト
https://docs.databricks.com/aws/ja/generative-ai/agent-framework/multi-agent-genie
