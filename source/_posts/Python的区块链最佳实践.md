---
title: Python的区块链最佳实践
date: 2018-04-28 08:16:04
tags: [区块链, Python]
---

Python的最佳区块链实践


==翻译的人说：==

**很喜欢区块链技术，但是技术能力有限，突然有一天，看到这篇介绍区块链技术的文章，一口气把他读完了，我这个小白终于搞明白了一点点东西。于是想把他翻译成中文，以飨国内读者。翻译版权归本人所有，如需转载，请联系我**

自从互联网开始以来到现在，区块链技术成为了目前是当之无愧的最具争议和火爆的一项技术。

在过去的这几年，由于加密数字货币的价值倍增，他作为比特币和其他加密货币的背后核心技术支撑吸引了无数眼球。

作为核心技术，区块链是一个分布式的数据库，他允许两个人做交易，而不需要中心化的机构。这个简单而又强大的概念会对诸于各种中心化的机构带来深远的影响和变革，比如银行，政府和市场等等。任何一个具有核心竞争的中心化的商业或者组织机构，都可以区块链技术所摧毁。

我们先不去管比特币或者其他数字货币所带来的金钱价值。这篇文章的目的是教会你用Python来做一个简单的区块链。第一部分和第二部分主要讲解了一些区块链技术的概念，到了第三部分，就开始来讲怎么用Python来实现区块链。为了便于理解，我们也会同时实现两个web页面，让用户能够很方便的来和区块链做一个交互。

请注意我这里是以比特币为例子来解释一个通用的”区块链技术”，所以这里所讲述的很多概念一样也适用于其他区块链技术或者加密数字货币

下面是我们在第三部分做的一个很生动的动态图。
  ![section]( http://jerry-yang.qiniudn.com/2018-04-26FuHxKL_y8-r5RJyv8aOPuom7FWP4)

# 1、区块链技术爆炸的开始
一切的一切，以一个自称中本聪的人，在2008年发表了一篇白皮书开始，这个白皮书的名字叫做，“比特币：一种点对点的加密电子货币交易系统”，然后就慢慢成为了大家现在所熟知的区块链技术，在原始的比特币论文中，中文聪阐释了如果建立一个点对点电子货币交易系统，可以让两个陌生人之间完直接成交易而不依赖于一个中心化的机构。这个系统解决了电子货币中一个非常重要的问题，那就是双重支付。



## 
1.1 什么是双重支付？
假设Alice想支付1美金给Bob，如果Alice使用纸币，那么在她跟Bob交易完之后，她手中的纸币将会从她的手中到Bob的手中。但是如果他们两个人都用电子货币，那么这个交易就变得复杂了。电子货币的形态决定了他可以很轻易的被复制。举个例子，假设Alice给Bob发一封邮件，里面的一个电子文件价值1美金。但是Bob不能确定Alice是否删除了她自己原有的那份文件的拷贝。如果Alice没有删除，并且将这份文件再发给Carol。这个问题就称之为双重支付。

![blockchain-double-spending](http://jerry-yang.qiniudn.com/2018-04-27FuffdixjHsU26P3vraJ8MTow2_DZ)

目前大多数解决这个双支问题的方法，就是在Alice，Bob，Carol等这些交易者之间建立一个第三方受信任的机构，比如银行。这个第三方机构负责管理一个中心账簿，然后跟踪和验证这个网络中的每一笔交易。对于这个系统，这种解决方案的缺点在于它需要一个受信任的第三方中介机构。

1.2 比特币：一个避免双支的去中心化的解决方案
为了解决双支问题，中本聪提出用一个公共账簿，比如，比特币的区块链是跟踪网络中所有的每一笔交易。比特币的区块链有以下特点：
- 分布式：账簿可以被任何电脑复制，而不是说存储在一个中心化的服务器上。任何只要连接了互联网的电脑都可以下载一个账簿副本。
- 加密：加密是用来保证发送者拥有她即将发送的比特币，以及决定这笔交易如何被加入到区块链中去。
- 不可改变：区块链只能以追加的形式更改。换句话说，任何交易只能被加入到区块链中，但是不能被删除或者修改。
- 工作量证明(PoW)：在网络中称之为矿工的一系列的特殊参与者，在开展一场竞赛，通过大量的哈希计算去破解一个加密谜题，谁先破解成功，系统就会允许他们加一个区块到比特币的区块链中。这个过程就称为工作量证明，这样可以让系统变得安全（关于这一点，后面还有更多讲解）
- 
![blockchain-cash-bitcoin](http://jerry-yang.qiniudn.com/2018-04-27FkQntR5Y4XVn_qPZZlsEquOONdf3)


发送比特币有如下步骤：
1. 第一步，创建一个比特币钱包，对一个人想发送或者接收比特币。他必须创建一个比特币钱包。一个比特币钱包存储了两个信息，一个私钥和一个公钥。私钥是一串密码字符串可以让一个用户发送比特币到另一个用户，或者在接受比特币支付的地方花掉比特币。公钥是一个字符串它能让用户接收比特币。公钥也可以说是比特币钱包地址（当然这样说不是很准，但是简单来讲，我们可以认为公钥和比特币钱包地址是一样的），请注意钱包并不存储比特币。所有有关比特币的信息都存储在比特币区块链中。
2. 创建一笔比特币交易，如果Alice想发送一枚比特币给Bob，Alice需要用她的私钥连接比特币网络，然后创建一笔里面含有比特币数量的交易，发送给对方的公钥（钱包）地址
3. 在整个网络中广播这条交易信息。一旦Alice创建了这笔交易，她需要再整个网络中广播这笔交易。
4. 确认这笔交易。某个矿工在比特币网络中监听并用Alice的公钥认证这笔交易，确认Alice的钱包里面有足够的比特币（至少要有1个）。然后在比特币区块链中添加这个交易详情的新记录。
5. 然后向所有的矿工广播，一旦交易被确认，那么矿工就应该向网络中的所有矿工广播这个区块链的变更信息，以确保他们的区块链副本保持同步。

# 区块链的深度研究
我们这部分的目标是深入研究区块链的技术。我们会介绍公钥密码学，哈希函数，区块链的挖掘以及其安全性。
2.1 公钥密码学
公钥密码学，或者换句话说，是非对称加密，是一个利用一对秘钥的加密系统，公钥可以对外公开，私钥只有自己知道。这个达到了两个目的，第一：鉴权。公钥是用来验证你是否是跟公钥配对的私钥所发送过来的消息，然后只有配对的私钥才能加密或者解密信息。
[RSA](https://en.wikipedia.org/wiki/RSA_(cryptosystem))  和 [ECDSA](https://en.wikipedia.org/wiki/Elliptic_Curve_Digital_Signature_Algorithm) 是最流行的公钥加密算法

在比特币里面，ECDSA用来生成钱包。比特币使用各种秘钥和地址，但是为了简单起见，在这篇文章中，我们假设每一个比特币钱包都有一对公私秘钥，以及比特币钱包地址就是公钥。如果你对比特币钱包的技术很感兴趣，我推荐这篇 [文章](https://en.bitcoin.it/wiki/Technical_background_of_version_1_Bitcoin_addresses)

为了发送或者接收比特币，用户首先必须创建已对公私秘钥。如果Alice想给Bob发送比特币，她需要创建一笔交易，并且填上Bob的公钥，以及她想发送多少比特币给Bob，她用她的私钥对这笔交易进行签名，一个在区块链上的电脑会用Alice的公钥去验证这笔交易，并将这笔交易添加到一个区块，然后随机这个区块会加入到整个区块链中。

![blockchain-public-crypto](http://jerry-yang.qiniudn.com/2018-04-27FovxdTlmdhZBEQBvad2po83GET57)

2.2 哈希函数和挖矿
所有的比特币交易都归集到文件里面并称之为区块。比特币每隔10分钟新增一个新的交易区快，一旦有新的区块加入到区块链中，那么这个区块开始不可被改变，以及不能被修改或者删除。网络中的一些特殊的参与者如矿工（链接在区块链中的电脑）负责为每笔交易创建新的区块。矿工用发送者的公钥去验证每一笔交易，确认发送者在此次交易中，发送者的钱包余额是充足的。然后将这笔交易添加到区块中去。矿工在选择将某一笔交易添加到区块中区是完全自由的，所以发送者必须要给矿工一点交易手续费，以激励矿工们将自己的交易添加到区块中去。

假如一个区块被成功添加到区块链中，他需要被挖出来。为了挖出一个新的区块，矿工们需要为这个密码难题找到一个答案。如果被挖到的区块被区块链所接受，那么矿工为得到一笔交易费，相当于一笔额外的激励吧。这个挖矿过程也可以说是工作量证明。他的主要机制就是让区块链变得可信和安全（更多关于区块链安全的在后面会讲到）

哈希和区块链解密难题
为了理解区块链密码难题，我们需要先从哈希函数开始。一个哈希函数就是可以将一个任意大小的数据转变为一个固定大小的数据。哈希函数的返回值称之为哈希。通常哈希函数通过查找重复记录来加速数据库的查询效率，当然，他们也广泛的应用于密码学。一个加密的哈希函数可以很轻易的对一个输入数据给出一个哈希值，但是如果输入数据是未知的，那么只知道哈希值是非常难得反向去还原出这串哈希值所对应的是什么数据的。

比特币所用的哈希函数叫做SHA-256.SHA-256用于区块数据之间的组合（比特币交易信息），和随机数。通过改变区块数据或者随机数，我们可以得到完全不同的哈希。所以当一个区块被确认为有效或者被矿工挖出，这个区块的哈希值或者随机数需要满足一个特定的条件。举个例子：开头的四个字符必须是0000，我们还可以增加更复杂的条件，必须不哈希必须满足前面8个都是0.

这个解密谜题就是矿工梦需要通过计算找出这样一个随机数以让这个哈希值满足这给条件。你可以用下面的app去模拟区块挖矿。当你在“Data”的输入框连输入数据或者改变随机数。你可以看见变化的哈希值。当你点击“Mine（挖矿）”，那么这个app开始生成一个以0开头的随机数，计算出哈希值然后检查开头四位是不是以4个0开头。如果开头四位不是4个0，他会给随机数加1，然后重复整个过程，直到找到满足4个0这个条件的随机数。如果这个区块被确认挖出，那么后台颜色会变成绿色。

模拟器：https://anders.com/blockchain/blockchain.html

2.3 从区块到区块链
在前面我们说到，交易信息是被分组到区块中，，然后区块是被追加到区块链中的。为了创造一个区块链。每一个区块是用到前一个区块的区块哈希作为当前区块的数据的一部分。为了创建一个新区快，矿工会选择一组交易，然后添加前一个区块的哈希值，然后继续像之前的形式一样进行挖矿。

任何区块中数据的改变会影响每一个区块的哈希值，如果没影响了，那么他们就变成无效的了。这给了区块链不可变更的特点。

![blockchain-from-blocks](http://jerry-yang.qiniudn.com/2018-04-27FrvqbZyw_K3xJoIAClYHhoMDC8GR)

你可以用这个app区模拟一个含有3个区块的区块链。当你在“Data”输入框输入数据和随机数的时候，你可以看到哈希值以及下一个区块的“前”一个哈希值。你可以通过点击Mine按钮来模拟更多的挖矿过程去创建独立的区块。在挖出三个区块只会，你可以尝试去改变区块1或者区块2的数据，你会看到所有的区块会变得无效。

[Blockchain Demo](https://anders.com/blockchain/blockchain.html)

上面的挖矿模拟来自Anders Brownworth’s牛逼的 [Blockchain Demo](https://anders.com/blockchain/blockchain.html)

2.3 添加区块到区块链
网络中所有的矿工都在参与一场计算竞赛，去找到一个有效的区块添加到区块链中区，从而能够得到一笔奖励。找到一个随机数去验证一个区块很难。因为矿工非常多，在网络中验证一个区块的可能性又是如此之大。第一个找到有效区块的矿工并添加到区块链中的会得到一笔比特币奖励，但是如果两个或者更过的矿工同时提交他们的区块呢？

# 解决冲突
如果两个矿工同时提交区块到区块链中，那么在网络中我们会得到两条不同的区块链，我们需要等到下一个区块去解决这个冲突。一些矿工会选择在区块链1上继续挖矿，另外一些矿工则选择在区块链2上继续挖矿。那么第一个找到新的区块解决冲突的，如果这个新的区块在区块链1上，那么区块链2就无效了。那么前一笔区块的这笔奖赏会给区块链1上的这名矿工。然后区块链2上的这笔交易不会被添加到主区块链上，然后会回滚到区块池，然后被添加到下一个区块中区。简而言之，如果区块链中存在冲突，那么最长的那一条链是赢家。

![blockchain-the-longest-chain-wins](http://jerry-yang.qiniudn.com/2018-04-27FgtsSPT85l7kwCx4gBqe3Ro-fODN)

2.5 区块链和双重支付（双花不会产生新的货币，只能把自己花出去的钱重新拿回来。）
在这一节，我们会介绍防止双支的最流行的方式，然后介绍用户如果保护自己免受伤害的一些方法。
Race Attack
一个人同时向网络中发送两笔交易，一笔交易发给自己（为了提高攻击成功率，他给这笔交易增加足够的小费），一笔交易发给商家。由于发送给自己的交易中含有较高的费，会被矿工打包成区块的概率比较高。为了防止这种攻击，最好是等到最后一个区块的确认信息出来后，再接受支付

Finney Attack
一个人挖到了一个区块（这个区块中包含一个交易 ：A向B转10BTC，其中A和B都是自己的地址），他先不广播这个区块，先找一个愿意接受未确认交易的商家向他购买一个物品，向商家发一笔交易：A向C转10BTC，付款后向网络中广播刚刚挖到的区块，由于区块中包含一个向自己付款的交易，所以他实现了一次双花。为了防止这种攻击，建议至少等待6个区块确认信息后再接受支付。

51%攻击
攻击者占有超过全网50%的算力，所以他可以创造一条高度大于原来链的新链。那么旧链中的交易会被回滚。攻击者可以发送一笔新的交易到新链上。这种公司概率很小，而且代价非常高昂。

3. 一个Python实现的区块链
在这一小节，我们将用Python实现一个基本的区块链和区块链客户端，我们的区块链有如下功能：
 - 可以添加很多节点到区块链中
 - 工作量证明
 - 节点之间简单的冲突解决
 - RSA加密交易信息
我们的区块链客户端有如下功能：
- 钱包用来生产公私钥
- 生成的交易用RSA加密

然后我们也同样做了两个Dashboard
1. 矿工的前端
2. 用户用来发送货币的钱包

这个区块链的实现很多事参考这个 [Github项目](https://github.com/dvf/blockchain)，我对源码做了一些改动，然后对交易进行了RSA加密，钱包生成和交易加密是基于这个 [Jupyter Notebook](https://github.com/julienr/ipynb_playground/blob/master/bitcoin/dumbcoin/dumbcoin.ipynb)   两个Dashboard是用html/css/js实现的，来自scratch的教学吧。

你可以下载整个源码，地址：https://github.com/adilmoujahid/blockchain-python-tutorial

请注意这个区块链的实现是为了教学目的，并不能用于生产，也没有什么安全上的考虑。并发也不高，然后很多重要功能没有实现。

3.1 区块链客户端实现
你可以开始启动一个客户端。首先进入blockchain_client文件夹，然后在命令行输入python blockchain_client.py，然后在浏览器打开http://localhost:8080，然后你就可以看到Dashboard了。

![blockchain-client](http://jerry-yang.qiniudn.com/2018-04-27FnAUiF735iyWs3sDtiWHZjbbPe4f)

Dashboard的导航栏有3个tab页面：
钱包生成器：利用RSA算法生成钱包地址，即公私钥。
做交易：生成一笔交易，然后将他们添加到区块链节点
查看交易信息：查看区块链上的交易信息

为了发起或者查看一笔交易，你同样需要启动区块链节点（将会在下一节介绍）

下面是一些关于blockchain_client.py代码的重要解释。

我们首先创建一个Python类，命名为Transaction，它有四个属性，sender_address, sender_private_key, recipient_address, value.这四个信息是发送者需要发起一笔交易的基本参数。

to_dict()方法以字典的形式返回交易信息（不包含发送者的私钥），签名函数是用发送者的私钥给交易信息（不包含发送者的私钥）签名的。
```Python
class Transaction:

    def __init__(self, sender_address, sender_private_key, recipient_address, value):
        self.sender_address = sender_address
        self.sender_private_key = sender_private_key
        self.recipient_address = recipient_address
        self.value = value

    def __getattr__(self, attr):
        return self.data[attr]

    def to_dict(self):
        return OrderedDict({'sender_address': self.sender_address,
                            'recipient_address': self.recipient_address,
                            'value': self.value})

    def sign_transaction(self):
        """
        Sign transaction with private key
        """
        private_key = RSA.importKey(binascii.unhexlify(self.sender_private_key))
        signer = PKCS1_v1_5.new(private_key)
        h = SHA.new(str(self.to_dict()).encode('utf8'))
        return binascii.hexlify(signer.sign(h)).decode('ascii')
```

 下面的是初始化一个Python  Flask对象，然后创建一些不同的API去在服务端与客户端做交互通信。

```python

app = Flask(__name__)

```

下面我们定义一个Flask路由，返回html页面。一个html页面对应一个tab。
 
```Python
@app.route('/')
def index():
  return render_template('./index.html')

@app.route('/make/transaction')
def make_transaction():
    return render_template('./make_transaction.html')

@app.route('/view/transactions')
def view_transaction():
    return render_template('./view_transactions.html')
```

下面我们顶一个API去生成钱包（公私钥）
```Python
@app.route('/wallet/new', methods=['GET'])
def new_wallet():
  random_gen = Crypto.Random.new().read
  private_key = RSA.generate(1024, random_gen)
  public_key = private_key.publickey()
  response = {
    'private_key': binascii.hexlify(private_key.exportKey(format='DER')).decode('ascii'),
    'public_key': binascii.hexlify(public_key.exportKey(format='DER')).decode('ascii')
  }

  return jsonify(response), 200
```

![blockchain-generate-wallet](http://jerry-yang.qiniudn.com/2018-04-27Ft0v5_ZIs-U5W83rniykh3PHD4-3)

 下面是我们定义一个输入sender_address, sender_private_key, recipient_address, value, 然后返回交易（不包含私钥）和签名
```Python
@app.route('/generate/transaction', methods=['POST'])
def generate_transaction():

  sender_address = request.form['sender_address']
  sender_private_key = request.form['sender_private_key']
  recipient_address = request.form['recipient_address']
  value = request.form['amount']

  transaction = Transaction(sender_address, sender_private_key, recipient_address, value)

  response = {'transaction': transaction.to_dict(), 'signature': transaction.sign_transaction()}

  return jsonify(response), 200
```


![blockchain-generate-transaction](http://jerry-yang.qiniudn.com/2018-04-27FufnlA6Bh08THnId3jVcVWcVRo6C)


3.2 你可以启动一个区块链的服务端，首先进入blockchain文件夹，然后在命令行执行python blockchain_client.py或者 python blockchain_client.py -p <PORT NUMBER>，如果你不指定一个端口号，将会用默认的5000作为端口号，然后打开浏览器http://localhost:<PORT NUMBER>，你就可以看到服务端的web界面

![blockchain-frontend](http://jerry-yang.qiniudn.com/2018-04-27FhlafIfeiPYUIqeGa0xlngdKOWpc)


这个Dashboard在导航栏有两个tab。
Mine：查看交易信息和区块链数据，以及对新的交易挖出新的区块
配置：配置不同的节点之间的连接
下面是对blockchain.py代码的解释

我们开始定义一个Blockchain的类，它包含如下属性：

transactions: 即将被添加到下一个区块中的交易列表

chain: 一个包含数组区块的真是区块链

nodes: 一组节点的链接，区块链用这些节点去获取其他节点上的数据然后同步节点信息

node_id: 一个区块链节点的id，是个随机数

Blockchain这个类实现了以下方法：

register_node(node_url): 添加节点到区块链中

verify_transaction_signature(sender_address, signature, transaction): 
用公钥检查对应签名的交易信息

submit_transaction(sender_address, recipient_address, value, signature): 
如果签名被确认过，则添加交易信息到一组交易信息列表里面

create_block(nonce, previous_hash): 添加交易信息到区块链中

hash(block): 创建一个sha-256的区块

proof_of_work(): Proof of work algorithm. 找到一个满足条件的随机数

valid_proof(transactions, last_hash, nonce, difficulty=MINING_DIFFICULTY): 
检查哈希值是否满足条件，这个函数使用的工作量证明函数

valid_chain(chain): 检查区块链是有效的

resolve_conflicts(): 解决冲突，用最长的那一条链去替换网络中的其他链
```Python
class Blockchain:

    def __init__(self):

        self.transactions = []
        self.chain = []
        self.nodes = set()
        #Generate random number to be used as node_id
        self.node_id = str(uuid4()).replace('-', '')
        #Create genesis block
        self.create_block(0, '00')

    def register_node(self, node_url):
        """
        Add a new node to the list of nodes
        """
        ...

    def verify_transaction_signature(self, sender_address, signature, transaction):
        """
        Check that the provided signature corresponds to transaction
        signed by the public key (sender_address)
        """
        ...

    def submit_transaction(self, sender_address, recipient_address, value, signature):
        """
        Add a transaction to transactions array if the signature verified
        """
        ...

    def create_block(self, nonce, previous_hash):
        """
        Add a block of transactions to the blockchain
        """
        ...

    def hash(self, block):
        """
        Create a SHA-256 hash of a block
        """
        ...

    def proof_of_work(self):
        """
        Proof of work algorithm
        """
        ...

    def valid_proof(self, transactions, last_hash, nonce, difficulty=MINING_DIFFICULTY):
        """
        Check if a hash value satisfies the mining conditions. This function is used within the proof_of_work function.
        """
        ...

    def valid_chain(self, chain):
        """
        check if a bockchain is valid
        """
        ...

    def resolve_conflicts(self):
        """
        Resolve conflicts between blockchain's nodes
        by replacing our chain with the longest one in the network.
        """
        ...
```

下面是Python的Flask框架初始化的对象，用来写一些API去和blockchain交互
```python
app = Flask(__name__)
CORS(app)
```

接下来我们将创建一个Blockchain实例
 
```Python 
blockchain = Blockchain()

``` 

我们顶一个两个API，用来返回服务端的Blockchain网页

```Python
@app.route('/')
def index():
    return render_template('./index.html')

@app.route('/configure')
def configure():
    return render_template('./configure.html')
```

下面是我们定义的API来管理交易和区块链中的挖矿
'/transactions/new': 这个接口是用着四个参数'sender_address', 'recipient_address', 'amount' and 'signature’, 如果签名是有效的话，然后添加每一笔交易到一组交易信息中，然后这组交易信息将会被添加到下一个区块中去
'/transactions/get': 这个接口是返回所有的即将被添加到下一个区块中的交易
'/chain': 这个接口返回所有的区块链数据
'/mine': 这个接口运行工作量证明，然后添加新的交易区块到区块链中

```Python
@app.route('/transactions/new', methods=['POST'])
def new_transaction():
    values = request.form

    # Check that the required fields are in the POST'ed data
    required = ['sender_address', 'recipient_address', 'amount', 'signature']
    if not all(k in values for k in required):
        return 'Missing values', 400
    # Create a new Transaction
    transaction_result = blockchain.submit_transaction(values['sender_address'], values['recipient_address'], values['amount'], values['signature'])

    if transaction_result == False:
        response = {'message': 'Invalid Transaction!'}
        return jsonify(response), 406
    else:
        response = {'message': 'Transaction will be added to Block '+ str(transaction_result)}
        return jsonify(response), 201

@app.route('/transactions/get', methods=['GET'])
def get_transactions():
    #Get transactions from transactions pool
    transactions = blockchain.transactions

    response = {'transactions': transactions}
    return jsonify(response), 200

@app.route('/chain', methods=['GET'])
def full_chain():
    response = {
        'chain': blockchain.chain,
        'length': len(blockchain.chain),
    }
    return jsonify(response), 200

@app.route('/mine', methods=['GET'])
def mine():
    # We run the proof of work algorithm to get the next proof...
    last_block = blockchain.chain[-1]
    nonce = blockchain.proof_of_work()

    # We must receive a reward for finding the proof.
    blockchain.submit_transaction(sender_address=MINING_SENDER, recipient_address=blockchain.node_id, value=MINING_REWARD, signature="")

    # Forge the new Block by adding it to the chain
    previous_hash = blockchain.hash(last_block)
    block = blockchain.create_block(nonce, previous_hash)

    response = {
        'message': "New Block Forged",
        'block_number': block['block_number'],
        'transactions': block['transactions'],
        'nonce': block['nonce'],
        'previous_hash': block['previous_hash'],
    }
    return jsonify(response), 200
```

![blockchain-mine](http://jerry-yang.qiniudn.com/2018-04-27FuJ-M_fn1uvdAvnyPGmVrh3jvnQL)

下面我们定义了一些接口来管理区块链节点
'/nodes/register': 这个接口接收一组节点url，然后添加他们到节点列表里面去
'/nodes/resolve': 这个接口是解决冲突用的，用最长的那条链替换其他链
'/nodes/get': 这个api返回节点信息

```python
@app.route('/nodes/register', methods=['POST'])
def register_nodes():
    values = request.form
    nodes = values.get('nodes').replace(" ", "").split(',')

    if nodes is None:
        return "Error: Please supply a valid list of nodes", 400

    for node in nodes:
        blockchain.register_node(node)

    response = {
        'message': 'New nodes have been added',
        'total_nodes': [node for node in blockchain.nodes],
    }
    return jsonify(response), 201


@app.route('/nodes/resolve', methods=['GET'])
def consensus():
    replaced = blockchain.resolve_conflicts()

    if replaced:
        response = {
            'message': 'Our chain was replaced',
            'new_chain': blockchain.chain
        }
    else:
        response = {
            'message': 'Our chain is authoritative',
            'chain': blockchain.chain
        }
    return jsonify(response), 200


@app.route('/nodes/get', methods=['GET'])
def get_nodes():
    nodes = list(blockchain.nodes)
    response = {'nodes': nodes}
    return jsonify(response), 200
```

![blockchain-manage-nodes]( http://jerry-yang.qiniudn.com/2018-04-27FshJJv4_YBPPQVmZ_i4J-byteXg7)
 结论：
在这篇博客，我们介绍了一些区块链背后的核心概念，以及我们学习到了怎么样使用Python来实现一个区块链。为了求简单，我们略过了很多技术细节，比如：钱包地址hash树。如果你想在这个课题上有更多的研究，我建议你读一下比特币的白皮书然后跟着 [Bitcoin wiki](https://en.bitcoin.it/wiki/Main_Page) ,以及 Andreas Antonopoulos's的牛逼哄哄的书，[Mastering Bitcoin: Programming the Open Blockchain.](https://www.amazon.com/gp/product/1491954388/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=1491954388&linkCode=as2&tag=adilmoujahid-20&linkId=bd776f9224715e8a022d4984909d6a69)

参考：

1 - [Wikipedia - Public-key cryptography](https://en.wikipedia.org/wiki/Public-key_cryptography)

2 - [Wikipedia - Hash function](https://en.wikipedia.org/wiki/Hash_function)

3 - [Bitcoin Stackexchange - What happens to a transaction once generated?](https://bitcoin.stackexchange.com/questions/58687/what-happens-to-a-transaction-once-generated)

4 - [Bitcoin Wiki - Majority attack](https://en.bitcoin.it/wiki/Majority_attack)

 翻译来自：
 http://adilmoujahid.com/posts/2018/03/intro-blockchain-bitcoin-python