---
layout: post
title: ChatGPT API 介绍
categories: [ai, chatGPT]
description: SwiftUI is a modern way to declare user interfaces for any Apple platform. Create beautiful, dynamic apps faster than ever before.
keywords: ai, GPT, chat

---

### [快速开始](https://platform.openai.com/docs/quickstart/quickstart)

OpenAI 训练了非常擅长理解和生成文本的尖端语言模型。我们的 API 提供对这些模型的访问，可用于解决几乎任何涉及处理语言的任务。

在本快速入门教程中，您将构建一个简单的示例应用程序。在此过程中，您将学习使用 API 完成任何任务的关键概念和技术，包括：

- 内容生成
- 总结
- 分类、分类和情感分析
- 数据提取
- 翻译
- 还有更多！

### [介绍](https://platform.openai.com/docs/quickstart/introduction)

完成端点是我们 API 的核心，它提供了一个极其灵活和强大的简单接口[。](https://platform.openai.com/docs/api-reference/completions)您输入一些文本作为**提示**，API 将返回一个文本**完成**，尝试匹配您提供的任何指令或上下文。

提示 ：`为冰淇淋店写一个标语。`

完成：`每一勺我们都会微笑！`

您可以将其视为非常高级的自动完成——模型处理您的文本提示并尝试预测接下来最有可能出现的内容。

#### [1从指令开始](https://platform.openai.com/docs/quickstart/start-with-an-instruction)

假设您想创建一个宠物名字生成器。从头开始想出名字很难！

首先，您需要一个明确说明您想要什么的提示。让我们从一个指令开始。**提交此提示**以生成您的第一个完成。

‍

不错！现在，试着让你的指示更具体。

‍

如您所见，在我们的提示中添加一个简单的形容词会改变生成的完成。设计提示本质上就是您“编程”模型的方式。

[2添加一些例子](https://platform.openai.com/docs/quickstart/add-some-examples)

制定好的说明对于获得好的结果很重要，但有时它们还不够。让我们试着让你的指令更复杂。

‍

这个完成并不是我们想要的。这些名称非常通用，而且模型似乎没有接受我们指令中的马匹部分。让我们看看能否让它提出一些更相关的建议。

在许多情况下，向模型展示*和*告诉模型您想要什么是很有帮助的。在您的提示中添加示例可以帮助传达模式或细微差别。尝试提交此提示，其中包含几个示例。

‍

好的！添加我们期望给定输入的输出示例有助于模型提供我们正在寻找的名称类型。

[3调整您的设置](https://platform.openai.com/docs/quickstart/adjust-your-settings)

提示设计并不是您可以使用的唯一工具。您还可以通过调整设置来控制完成。最重要的设置之一称为**温度**。

您可能已经注意到，如果您在上面的示例中多次提交相同的提示，模型将始终返回相同或非常相似的完成。这是因为您的温度设置为**0**。

**尝试将 temperature 设置为1**重新提交几次相同的提示。

‍

温度

看看发生了什么？当温度高于 0 时，每次提交相同的提示会导致不同的完成。

请记住，该模型预测哪个文本最有可能跟在它前面的文本之后。温度是一个介于 0 和 1 之间的值，基本上可以让您控制模型在进行这些预测时的置信度。降低温度意味着它将承担更少的风险，并且完成将更加准确和确定。升高温度将导致更多样化的完成。



深潜

了解标记和概率



对于您的昵称生成器，您可能希望能够生成很多名字创意。0.6 的适中温度应该可以正常工作。

[4构建您的应用程序](https://platform.openai.com/docs/quickstart/build-your-application)

节点.JS蟒蛇（烧瓶）

现在您已经找到了一个好的提示和设置，您已经准备好构建您的爱称生成器了！我们编写了一些代码来帮助您入门 - 按照以下说明下载代码并运行应用程序。

[设置](https://platform.openai.com/docs/quickstart/setup)

如果您没有安装 Node.js，[请从此处安装](https://nodejs.org/en/)。[然后通过克隆此存储库](https://github.com/openai/openai-quickstart-node)下载代码。



```text
git clone https://github.com/openai/openai-quickstart-node.git
```

[如果您不想使用 git，您也可以使用此 zip 文件](https://github.com/openai/openai-quickstart-node/archive/refs/heads/master.zip)下载代码。

[添加您的 API 密钥](https://platform.openai.com/docs/quickstart/add-your-api-key)

导航到项目目录并复制示例环境变量文件。



```text
cd openai-quickstart-node
cp .env.example .env
```

复制您的秘密 API 密钥并将其设置为`OPENAI_API_KEY`您新创建的`.env`文件中的。如果您还没有创建密钥，您可以在下面创建。

您目前没有任何 API 密钥。请在下面创建一个。

创建新密钥



**重要说明：使用 Javascript 时，所有 API 调用都应仅在服务器端进行，因为在客户端浏览器代码中进行调用会暴露您的 API 密钥。有关更多详细信息，[请参见此处。](https://platform.openai.com/docs/api-reference/authentication)**

[运行应用](https://platform.openai.com/docs/quickstart/run-the-app)

在项目目录下运行以下命令安装依赖并运行应用程序。



```text
npm install
npm run dev
```

在浏览器中打开[http://localhost:3000](http://localhost:3000/)，您应该会看到昵称生成器！

[理解代码](https://platform.openai.com/docs/quickstart/understand-the-code)

`generate.js`在文件夹中打开`openai-quickstart-node/pages/api`。在底部，您将看到生成我们在上面使用的提示的函数。由于用户将输入他们宠物的动物类型，因此它会动态换出指定动物的提示部分。



```javascript
1
2
3
4
5
6
7
8
9
10
11
function generatePrompt(animal) {
  const capitalizedAnimal = animal[0].toUpperCase() + animal.slice(1).toLowerCase();
  return `Suggest three names for an animal that is a superhero.

Animal: Cat
Names: Captain Sharpclaw, Agent Fluffball, The Incredible Feline
Animal: Dog
Names: Ruff the Protector, Wonder Canine, Sir Barks-a-Lot
Animal: ${capitalizedAnimal}
Names:`;
}
```

在 中的第 9 行`generate.js`，您将看到发送实际 API 请求的代码。如上所述，它使用温度为 0.6 的[完成端点。](https://platform.openai.com/docs/api-reference/completions)



```javascript
1
2
3
4
5
const completion = await openai.createCompletion({
  model: "text-davinci-003",
  prompt: generatePrompt(req.body.animal),
  temperature: 0.6,
});
```

就是这样！您现在应该完全了解您的（超级英雄）宠物名称生成器如何使用 OpenAI API！

[关闭](https://platform.openai.com/docs/quickstart/closing)

这些概念和技术将大大有助于您构建自己的应用程序。也就是说，这个简单的例子只是展示了可能性的一小部分！完成端点非常灵活，几乎可以解决任何语言处理任务，包括内容生成、摘要、语义搜索、主题标记、情感分析等等。

要记住的一个限制是，对于大多数**模型**，单个 API 请求在提示和完成之间最多只能处理 2,048 个标记（大约 1,500 个单词）。



深潜

型号和定价



对于更高级的任务，您可能会发现自己希望能够提供更多的示例或上下文，而不是单个提示中的内容。微调[API](https://platform.openai.com/docs/guides/fine-tuning)是执行此类更高级任务的绝佳选择。**微调**允许您提供数百甚至数千个示例来为您的特定用例定制模型。

[下一步](https://platform.openai.com/docs/quickstart/next-steps)

要获得灵感并了解有关为不同任务设计提示的更多信息：

- 阅读我们的[完成指南](https://platform.openai.com/docs/guides/completion)。
- 浏览我们的[示例提示](https://platform.openai.com/examples)库。
- 开始在[Playground](https://platform.openai.com/playground)中进行试验。
- 在开始构建时，请牢记我们的[使用政策。](https://platform.openai.com/docs/usage-policies)
