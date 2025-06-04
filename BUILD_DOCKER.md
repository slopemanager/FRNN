
# Build (GPU) Docker Image for `frnn` and `prefix-sum`


Build `frnn` and `prefix-sum` using `pytorch/pytorch:2.2.0-cuda12.1-cudnn8-devel`. Both wheels will be available in the `dist` folder.

```bash
# Prepare builder image
docker build --pull --rm -f 'dockerfile.gpu' -t 'frnn-builder:latest' '.'

# Build frnn
docker run --gpus all -it --rm -v ${pwd}:/app -w /app frnn-builder:latest bash -c "python setup.py bdist_wheel"

# Build prefix_sum
docker run --gpus all -it --rm -v ${pwd}:/app -w /app frnn-builder:latest bash -c "cd /app/external/prefix_sum && python setup.py bdist_wheel --dist-dir /app/dist"
```


Run and test using `pytorch/pytorch:2.2.0-cuda12.1-cudnn8-runtime`:

```bash
docker run --gpus all -it --rm -v ${pwd}:/app -w /app pytorch/pytorch:2.2.0-cuda12.1-cudnn8-runtime bash

# Install frnn and prefix_sum
pip install /app/dist/prefix_sum-0.0.0-cp310-cp310-linux_x86_64.whl
pip install /app/dist/frnn-0.0.0-cp310-cp310-linux_x86_64.whl
```