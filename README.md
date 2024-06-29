# Pre-trian steps

## Activate conda enviroment
```
conda activate llama-factory
```

## 1.Prepare datasets
Read './data_preprocess.ipynb', Save `bioenv.json` in `./data`

## 2.Edit `LLaMA-Factory/data/dataset_info.json`
Add dataset {Key:Value} into `dataset_info.json`
```
  "bioenv": {
    "file_name": "bioenv.json",
    "columns": {
      "prompt": "text"
    }
  }
```

## 3. Pre-train
1. Go to `examples/lora_single_gpu`
2. The pretrained examples of llama3 is `llama3_lora_pretrain.yaml`, duplicate it and edit you want to use.
3. Run command below
```
CUDA_VISIBLE_DEVICES=0 llamafactory-cli train examples/lora_single_gpu/breeze_7b_instruct_lora_pretrain.yaml
```

## 4. Merging LoRA Adapters and Quantization
```
# Merging head
CUDA_VISIBLE_DEVICES=0 llamafactory-cli export examples/merge_lora/breeze_7b_instruct_lora_sft.yaml
# Quantization
CUDA_VISIBLE_DEVICES=0 llamafactory-cli export examples/merge_lora/breeze_7b_instruct_gptq.yaml
# 14GB -> 4.4GB
```

## Inferring LoRA Fine-Tuned Models
Run below on command or see chat.ipynb.
```
# CLI
CUDA_VISIBLE_DEVICES=0 llamafactory-cli chat examples/inference/breeze_7b_instruct_gptq.yaml
```