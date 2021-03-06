# 过拟合与欠拟合

[Colab notebook](https://colab.research.google.com/github/tensorflow/models/blob/master/samples/core/tutorials/keras/overfit_and_underfit.ipynb)

# To add a new cell, type '# %%'
# To add a new markdown cell, type '# %% [markdown]'
# %% [markdown]
# ##### Copyright 2018 The TensorFlow Authors.

# %%
#@title Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
# https://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.


# %%
#@title MIT License
#
# Copyright (c) 2017 François Chollet
#
# Permission is hereby granted, free of charge, to any person obtaining a
# copy of this software and associated documentation files (the "Software"),
# to deal in the Software without restriction, including without limitation
# the rights to use, copy, modify, merge, publish, distribute, sublicense,
# and/or sell copies of the Software, and to permit persons to whom the
# Software is furnished to do so, subject to the following conditions:
#
# The above copyright notice and this permission notice shall be included in
# all copies or substantial portions of the Software.
#
# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
# IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
# FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL
# THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
# LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING
# FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER
# DEALINGS IN THE SOFTWARE.

# %% [markdown]
# # 探索过拟合与欠拟合
# %% [markdown]
# <table class="tfo-notebook-buttons" align="left">
#   <td>
#     <a target="_blank" href="https://www.tensorflow.org/tutorials/keras/overfit_and_underfit"><img src="https://www.tensorflow.org/images/tf_logo_32px.png" />View on TensorFlow.org</a>
#   </td>
#   <td>
#     <a target="_blank" href="https://colab.research.google.com/github/tensorflow/docs/blob/master/site/en/tutorials/keras/overfit_and_underfit.ipynb"><img src="https://www.tensorflow.org/images/colab_logo_32px.png" />Run in Google Colab</a>
#   </td>
#   <td>
#     <a target="_blank" href="https://github.com/tensorflow/docs/blob/master/site/en/tutorials/keras/overfit_and_underfit.ipynb"><img src="https://www.tensorflow.org/images/GitHub-Mark-32px.png" />View source on GitHub</a>
#   </td>
# </table>
# %% [markdown]
# 与往常一样，此示例中的代码将使用 tf.keras API，您可以在 TensorFlow [Keras 指南](https://www.tensorflow.org/guide/keras)中了解更多信息。
# 
# 在之前的两个例子中 — 电影评论文本分类和预测住房价格 — 我们看到模型在验证数据上的准确率在经过多个 epoch 训练后会达到峰值，然后开始下降。 
# 
# 换句话说，我们的模型在训练数据上<b>过拟合</b>了。学习如何处理过度拟合很重要。虽然可以在<b>训练集</b>上实现很高准确率，但我们真正需要的是开发出在<b>测试数据</b>（或者从未出现过的数据）同样表现较好的模型。
# 
# 过拟合相反的情况是<b>欠拟合</b>。当在测试数据上仍有提升空间时，意味着模型欠拟合。出现这种情况的原因有很多：如果模型不够强大，过度正则化，或者根本没有经过足够长时间的训练。这意味着网络还没有学习到训练数据中的相关特征。 
# 
# 如果训练时间过长，模型将开始过拟合并从训练数据中学习到一些在测试数据上不具有泛化能力的特征。我们需要权衡模型训练的时间。我们将在后面学习如何控制模型训练的 epoch 数量，这对我们来说是一个很重要的技能。
# 
# 为了防止过拟合，最好的解决方案是使用更多的训练数据。使用大量数据训练的模型泛化能力会更好。当数据量不足时，下一个最佳解决方案是使用正规化等技术。这些限制了模型可以存储信息的数量和类型。 如果一个网络只能记住少量的模式，那么优化过程将迫使它专注于最突出的模式，这些模式有更好的概括性。
# 
# 在这个笔记本中，我们将探索两种常见的正则化技术 — 权重正则化和丢失 — 并使用它们来改进我们的 IMDB 电影评论文本分类。

# %%
import tensorflow as tf
from tensorflow import keras

import numpy as np
import matplotlib.pyplot as plt

print(tf.__version__)

# %% [markdown]
# ## 下载 IMDB 数据集
# 
# 我们不会使用之前笔记本中提到的嵌入向量，而是对句子进行多重编码。该模型将很快在训练集上发生过拟合。它将用于证明何时发生过度拟合，以及如何避免。 
# 
# 对数据进行热编码意味着将它们转换为只包含 0 和 1 的向量。具体地说，如果要将序列 `[3, 5]` 转换为10,000维向量，那么意味着除索引 3 和 5 是数字 1，其余都是 0。

# %%
NUM_WORDS = 10000

(train_data, train_labels), (test_data, test_labels) = keras.datasets.imdb.load_data(num_words=NUM_WORDS)

def multi_hot_sequences(sequences, dimension):
    # Create an all-zero matrix of shape (len(sequences), dimension)
    results = np.zeros((len(sequences), dimension))
    for i, word_indices in enumerate(sequences):
        results[i, word_indices] = 1.0  # set specific indices of results[i] to 1s
    return results


train_data = multi_hot_sequences(train_data, dimension=NUM_WORDS)
test_data = multi_hot_sequences(test_data, dimension=NUM_WORDS)

# %% [markdown]
# 让我们看一下生成的热编码向量。单词索引按频率排序，因此预计索引零附近有更多的 1 值，我们可以在此图中看到：

# %%
plt.plot(train_data[0])

# %% [markdown]
# ## 验证过拟合
# 
# 防止过度拟合的最简单方法是减小模型大小，即模型中需要学习的参数的数量（由层数和每层单元数决定）。在深度学习中，模型中参数数量通常被称为模型的「容量」。直观地，具有更多参数的模型将具有更多「记忆能力」，因此将能够容易地学习训练样本与其目标之间的完美的映射关系，但是该映射没有任何泛化能力，因此在未知的数据上预测没有任何意义。 
# 
# 始终记住一点：深度学习模型擅长拟合训练数据，但真正的挑战是泛化，而不是拟合。
# 
# 另一方面，如果网络具有有限的记忆资源，则将不能容易地学习映射。为了最大限度地减少损失，它必须学习具有更强预测能力的压缩表示。同时，如果您使模型太小，则难以拟合训练数据。需要在「容量过大」和「容量不足」之间保持平衡。
# 
# 不幸的是，没有神奇的公式来确定模型的大小或架构（就层数或每层的单元数量而言）。您将不得不尝试使用一系列不同的架构。
# 
# 要找到合适的模型大小，最好从相对较少的网路层和参数开始，然后开始增加网络层的大小或添加新网络层，直到您看到在验证集上的损失递减为止。让我们在电影评论分类网络上试试。 
# 
# 我们将仅使用```全连接```图层作为基准创建一个简单的模型，然后创建更小和更大的版本，并进行比较。
# %% [markdown]
# ### 创建基准模型

# %%
baseline_model = keras.Sequential([
    # `input_shape` is only required here so that `.summary` works. 
    keras.layers.Dense(16, activation=tf.nn.relu, input_shape=(NUM_WORDS,)),
    keras.layers.Dense(16, activation=tf.nn.relu),
    keras.layers.Dense(1, activation=tf.nn.sigmoid)
])

baseline_model.compile(optimizer='adam',
                       loss='binary_crossentropy',
                       metrics=['accuracy', 'binary_crossentropy'])

baseline_model.summary()


# %%
baseline_history = baseline_model.fit(train_data,
                                      train_labels,
                                      epochs=20,
                                      batch_size=512,
                                      validation_data=(test_data, test_labels),
                                      verbose=2)

# %% [markdown]
# ### 创建更小模型
# %% [markdown]
# 让我们创建一个隐藏单元较少的模型，与我们刚刚创建的基准模型进行比较：

# %%
smaller_model = keras.Sequential([
    keras.layers.Dense(4, activation=tf.nn.relu, input_shape=(NUM_WORDS,)),
    keras.layers.Dense(4, activation=tf.nn.relu),
    keras.layers.Dense(1, activation=tf.nn.sigmoid)
])

smaller_model.compile(optimizer='adam',
                loss='binary_crossentropy',
                metrics=['accuracy', 'binary_crossentropy'])

smaller_model.summary()

# %% [markdown]
# 在相同的数据集上训练：

# %%
smaller_history = smaller_model.fit(train_data,
                                    train_labels,
                                    epochs=20,
                                    batch_size=512,
                                    validation_data=(test_data, test_labels),
                                    verbose=2)

# %% [markdown]
# ### 创建更大的模型
# 
# 作为练习，您可以创建一个更大的模型，并查看它开始过拟合的速度。接下来，让我们在这个基准测试中添加一个容量大得多的网络，远远超出问题的范围：

# %%
bigger_model = keras.models.Sequential([
    keras.layers.Dense(512, activation=tf.nn.relu, input_shape=(NUM_WORDS,)),
    keras.layers.Dense(512, activation=tf.nn.relu),
    keras.layers.Dense(1, activation=tf.nn.sigmoid)
])

bigger_model.compile(optimizer='adam',
                     loss='binary_crossentropy',
                     metrics=['accuracy','binary_crossentropy'])

bigger_model.summary()

# %% [markdown]
# 同样在相同的数据集上训练模型：

# %%
bigger_history = bigger_model.fit(train_data, train_labels,
                                  epochs=20,
                                  batch_size=512,
                                  validation_data=(test_data, test_labels),
                                  verbose=2)

# %% [markdown]
# ### 绘制训练集和验证机损失
# 
# <!--TODO(markdaoust): This should be a one-liner with tensorboard -->
# %% [markdown]
# 实线表示训练损失，虚线表示验证损失（记住：较低的验证损失表示更好的模型）。在这里，较小的网络过拟合时间晚于基线模型（在 6 个 epoch 之后而不是 4 个 epoch），并且一旦开始过度拟合，其性能下降得慢得多。

# %%
def plot_history(histories, key='binary_crossentropy'):
  plt.figure(figsize=(16,10))
    
  for name, history in histories:
    val = plt.plot(history.epoch, history.history['val_'+key],
                   '--', label=name.title()+' Val')
    plt.plot(history.epoch, history.history[key], color=val[0].get_color(),
             label=name.title()+' Train')

  plt.xlabel('Epochs')
  plt.ylabel(key.replace('_',' ').title())
  plt.legend()

  plt.xlim([0,max(history.epoch)])


plot_history([('baseline', baseline_history),
              ('smaller', smaller_history),
              ('bigger', bigger_history)])

# %% [markdown]
# 请注意，较大的网络在仅仅一个 epoch 之后就开始过拟合，并且越来越严重。网络容量越大，能够越快地对训练数据进行建模（导致训练损失低），但过拟合的可能性越大（导致训练和验证损失之间的差异很大）。
# %% [markdown]
# ## 策略
# %% [markdown]
# ### 增加权重正则化
# 
# 
# %% [markdown]
# 你可能熟悉 Occam's Razor 原则：给出某个事件的两种解释，最可能正确的解释是「最简单」的解释，即做出最少假设的解释。这也适用于神经网络学习的模型：给定一些训练数据和网络架构，有多组权重值（多个模型）可以拟合数据，相比于复杂模型，简单模型不容易过拟合。
# 
# 在这种情况下，「简单模型」是一个模型，其中参数值的分布具有较小的熵（如我们在上面提到的具有较少参数的模型）。因此，降低过拟合的常见方法是通过限制网络权重，仅采用较小的权重值来约束网络的复杂性施，这使得权重值的分布更「规则」。这被称为「权重正则化」，并且通过向网络的损失函数添加与具有大权重相关联的代价来完成。这个代价有两种：
# 
# * L1 正则化，其中所添加的成本与权重系数的绝对值成比例（即权重的「L1 范数」）。
# 
# * L2 正则化，其中所添加的成本与权重系数值的平方成比例（即权重的所谓「L2 范数」）。L2 正则化在神经网络的背景下也称为权重衰减。不要让不同的名字让你感到困惑：权重衰减在数学上与 L2 正则化完全相同。
# 
# 在 `tf.keras` 中，通过将权重正则化实例作为关键参数传递给网络层来实现权重正则化。现在让我们添加 L2 权重正则化。

# %%
l2_model = keras.models.Sequential([
    keras.layers.Dense(16, kernel_regularizer=keras.regularizers.l2(0.001),
                       activation=tf.nn.relu, input_shape=(NUM_WORDS,)),
    keras.layers.Dense(16, kernel_regularizer=keras.regularizers.l2(0.001),
                       activation=tf.nn.relu),
    keras.layers.Dense(1, activation=tf.nn.sigmoid)
])

l2_model.compile(optimizer='adam',
                 loss='binary_crossentropy',
                 metrics=['accuracy', 'binary_crossentropy'])

l2_model_history = l2_model.fit(train_data, train_labels,
                                epochs=20,
                                batch_size=512,
                                validation_data=(test_data, test_labels),
                                verbose=2)

# %% [markdown]
# ```l2(0.001)``` 意味着权值矩阵中的每一个参数都要经过 ```0.001 * weight_coefficient_value**2``` 累加到神经网络的代价损失函数中。请注意，由于此惩罚仅在训练时添加，因此在训练时此网络的损失将远高于测试时的损失。
# 
# 下面给出 L2 正规化惩罚的影响：

# %%
plot_history([('baseline', baseline_history),
              ('l2', l2_model_history)])

# %% [markdown]
# 正如您所看到的，L2 正则化模型与基准模型相比能够避免过拟合，即使两个模型具有相同数量的参数。
# %% [markdown]
# ### 添加 Dropout
# 
# Dropout 是由 Hinton 和他在多伦多大学的学生开发的最有效和最常用的神经网络正则化技术之一。在训练期间随机「丢弃」（即设置为零）该层的多个输出值。假设一个给定的层在训练期间为给定的输入样本返回一个向量[0.2,0.5,1.3,0.8,1.1]；在应用了 Dropout 之后，该向量部分值被随机置为 0，例如，[0,0.5，
# 1.3,0,1.1]。「dropout 比例」是丢弃特征的比例；它通常取值在 0.2 和 0.5 之间。在测试时，没有单位被丢弃，而是将图层的输出值按比例缩小并等于 Dropout 比例，以便平衡更多单位活跃的事实而不是训练时间。
# 
# 在 tf.keras 中，您可以通过 Dropout 图层在网络中引入 Dropout，该图层应用于网络层输出之前。
# 
# 让我们在 IMDB 网络中添加两个 Dropout 图层，看看它们在降低过拟合方面做得如何：

# %%
dpt_model = keras.models.Sequential([
    keras.layers.Dense(16, activation=tf.nn.relu, input_shape=(NUM_WORDS,)),
    keras.layers.Dropout(0.5),
    keras.layers.Dense(16, activation=tf.nn.relu),
    keras.layers.Dropout(0.5),
    keras.layers.Dense(1, activation=tf.nn.sigmoid)
])

dpt_model.compile(optimizer='adam',
                  loss='binary_crossentropy',
                  metrics=['accuracy','binary_crossentropy'])

dpt_model_history = dpt_model.fit(train_data, train_labels,
                                  epochs=20,
                                  batch_size=512,
                                  validation_data=(test_data, test_labels),
                                  verbose=2)


# %%
plot_history([('baseline', baseline_history),
              ('dropout', dpt_model_history)])

# %% [markdown]
# 添加 Dropout 对基准模型有明显改进。 
# 
# 
# 回顾一下：这里介绍了防止神经网络中过拟合的常见方法：
# 
# * 获取更多的训练数据。
# * 减少网络的容量。
# * 权重正则化。
# * 添加 Dropout。
# 
# 本指南未涉及的两个重要方法是数据增强和批量标准化。
