# Energon-AI for Bloom inference
# How to run
To start service
```
bash run.sh
```

You can change the args in `run.sh` as follow:
```
 param list
 --name :Name Path (required)
 --tp: (int) GPU_NUM, default=1
 --http_host: (x.x.x.x)  your IP address, default=0.0.0.0
 --http_port: (xxxx) your port, default=7070
 --dtype:(str) use int8-quant or not ["fp16", "int8"], default="fp16"
 --max_batchsize:(int) limitation of batchsize, default=1
 --random_init:(bool) random init or not(if you don't have whole model data), default=False
 --random_model_size:(str) size of random init model,["560m", "7b1", "175b"],default="560m"

Once use [--random_init True], the [--random_model_size] option will be used. 
Name is also required while using[--random_init True], for getting tokenizer. 
```

While the service is running, send `POST` requests to https://[ip]:[port]/generation     

send POST body as json file:
```
curl -X POST http://ip:port/generation -H "Content-Type: application/json" -d @test.json
```

test.json looks like follows:
```
{
    "prompt": "However, there are still immense benefits to learning quantum computing.",
    "top_p": 0.90,
    "top_k": 40,
    "max_new_tokens": 60
}
```  

received message: 
```
{
    "text": "However, there are still immense benefits to learning quantum computing. For example, quantum computing can be used to solve problems that are difficult to solve by classical methods. Quantum computing can also be used to solve problems that are difficult to solve by classical methods. Quantum computing can also be used to solve problems that are"
}
```

# Configure
## Configure batching
add `--max_batch_size <MaxBatchSize>`  to the python command in `run.sh`

The `<MaxBatchSize>` can be an integer in `[1, MAXINT]`. The engine will make batch whose size is less or equal to this value.

Bigger MaxBatchSize may speed up concurrent requests in a single batched forwarding process.

# Testing Result
## Memory
Int8_model_size = 1/2 FP16_model_size
To inference the 175B Bloom model with 8 GPUs, we reduce the MAX_GPU_MEM_ALLOCATED from 85GB per GPU to 21.68GB per GPU !
Also we use no more cpu_mem than fp16 model.
## Time
TODO

