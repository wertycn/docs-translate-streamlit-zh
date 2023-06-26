---
标题: st.camera_input
别名: /library/api-reference/widgets/st.camera_input
描述: st.camera_input显示一个用于从摄像头上传图像的小部件
---

<Autofunction function="streamlit.camera_input" />

要将图像文件缓冲区作为字节读取，可以在`UploadedFile`对象上使用`getvalue()`方法。

```python
import streamlit as st

img_file_buffer = st.camera_input("Take a picture")

if img_file_buffer is not None:
    # To read image file buffer as bytes:
    bytes_data = img_file_buffer.getvalue()
    # Check the type of bytes_data:
    # Should output: <class 'bytes'>
    st.write(type(bytes_data))
```

<重要>

`st.camera_input` 返回一个 `UploadedFile` 类的对象，它是 `BytesIO` 的子类。因此它是一个"类似文件"的对象。这意味着您可以将它传递到任何需要文件的地方，类似于 `st.file_uploader`。

</重要>

## 图像处理示例

您可以使用`st.camera_input`的输出进行各种下游任务，包括图像处理。下面，我们将演示如何在流行的图像和数据处理库（如[Pillow](https://pillow.readthedocs.io/en/stable/installation.html)、[NumPy](https://numpy.org/)、[OpenCV](https://pypi.org/project/opencv-python-headless/)、[TensorFlow](https://www.tensorflow.org/)、[torchvision](https://pytorch.org/vision/stable/index.html)和[PyTorch](https://pytorch.org/)）中使用`st.camera_input`小部件。

虽然我们为最常见的用例和库提供了示例，但您可以根据自己的需求和喜好的库进行调整。

### Pillow (PIL) 和 NumPy

确保您已经安装了 [Pillow](https://pillow.readthedocs.io/en/stable/installation.html) 和 [NumPy](https://numpy.org/)。

要将图像文件缓冲区读取为 PIL 图像并将其转换为 NumPy 数组:

```python
import streamlit as st
from PIL import Image
import numpy as np

img_file_buffer = st.camera_input("Take a picture")

if img_file_buffer is not None:
    # To read image file buffer as a PIL Image:
    img = Image.open(img_file_buffer)

    # To convert PIL Image to numpy array:
    img_array = np.array(img)

    # Check the type of img_array:
    # Should output: <class 'numpy.ndarray'>
    st.write(type(img_array))

    # Check the shape of img_array:
    # Should output shape: (height, width, channels)
    st.write(img_array.shape)
```

### OpenCV（cv2）

请确保您已安装[OpenCV](https://pypi.org/project/opencv-python-headless/)和[NumPy](https://numpy.org/)。

使用OpenCV读取图像文件缓冲区的方法：

```python
import streamlit as st
import cv2
import numpy as np

img_file_buffer = st.camera_input("Take a picture")

if img_file_buffer is not None:
    # To read image file buffer with OpenCV:
    bytes_data = img_file_buffer.getvalue()
    cv2_img = cv2.imdecode(np.frombuffer(bytes_data, np.uint8), cv2.IMREAD_COLOR)

    # Check the type of cv2_img:
    # Should output: <class 'numpy.ndarray'>
    st.write(type(cv2_img))

    # Check the shape of cv2_img:
    # Should output shape: (height, width, channels)
    st.write(cv2_img.shape)
```

### TensorFlow

请确保您已安装[TensorFlow](https://www.tensorflow.org/install/)。

要使用TensorFlow将图像文件缓冲区读取为一个三维的uint8张量：

```python
import streamlit as st
import tensorflow as tf

img_file_buffer = st.camera_input("Take a picture")

if img_file_buffer is not None:
    # To read image file buffer as a 3D uint8 tensor with TensorFlow:
    bytes_data = img_file_buffer.getvalue()
    img_tensor = tf.io.decode_image(bytes_data, channels=3)

    # Check the type of img_tensor:
    # Should output: <class 'tensorflow.python.framework.ops.EagerTensor'>
    st.write(type(img_tensor))

    # Check the shape of img_tensor:
    # Should output shape: (height, width, channels)
    st.write(img_tensor.shape)
```

### Torchvision

确保您已经安装了[Torchvision](https://pypi.org/project/torchvision/)（它不随PyTorch捆绑）和[PyTorch](https://pytorch.org/)。

要使用`torchvision.io`将图像文件缓冲区读取为一个3维的uint8张量：

```python
import streamlit as st
import torch
import torchvision

img_file_buffer = st.camera_input("Take a picture")

if img_file_buffer is not None:
    # To read image file buffer as a 3D uint8 tensor with `torchvision.io`:
    bytes_data = img_file_buffer.getvalue()
    torch_img = torchvision.io.decode_image(
        torch.frombuffer(bytes_data, dtype=torch.uint8)
    )

    # Check the type of torch_img:
    # Should output: <class 'torch.Tensor'>
    st.write(type(torch_img))

    # Check the shape of torch_img:
    # Should output shape: torch.Size([channels, height, width])
    st.write(torch_img.shape)
```

### PyTorch

请确保您已经安装了[PyTorch](https://pytorch.org/)和[NumPy](https://numpy.org/)。

使用PyTorch将图像文件缓冲区读取为一个3维的uint8张量：

```python
import streamlit as st
import torch
import numpy as np

img_file_buffer = st.camera_input("Take a picture")

if img_file_buffer is not None:
    # To read image file buffer as a 3D uint8 tensor with PyTorch:
    bytes_data = img_file_buffer.getvalue()
    torch_img = torch.ops.image.decode_image(
        torch.from_numpy(np.frombuffer(bytes_data, np.uint8)), 3
    )

    # Check the type of torch_img:
    # Should output: <class 'torch.Tensor'>
    st.write(type(torch_img))

    # Check the shape of torch_img:
    # Should output shape: torch.Size([channels, height, width])
    st.write(torch_img.shape)
```
