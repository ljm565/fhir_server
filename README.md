# FHIR


### Recent updates 📣
* *June 2025 (v0.0.1)*: 

&nbsp;

&nbsp;






<!-- ## Quick Starts 🚀
### Environment setup
We have to install PyTorch and other requirements. Please refer to more [detailed setup](./docs/1_getting_started.md) including Docker.
```bash
# PyTorch Install
pip3 install torch==2.5.1 torchvision==0.20.1 torchaudio==2.5.1 --index-url https://download.pytorch.org/whl/cu124

# Requirements Install
pip3 install -r docker/requirements.txt
```

&nbsp;

### Data preparation
```bash
python3 src/run/dataset_download.py --dataset allenai/ai2_arc --download_path data_examples
```

&nbsp;

### LLM training
```bash
# Llama 3.1 8B LoRA fine-tuning
python3 src/run/train.py --config config/example_llama3.1_lora.yaml --mode train

# Llama 3.1 8B QLoRA fine-tuning
python3 src/run/train.py --config config/example_llama3.1_qlora.yaml --mode train

# Llama 3.1 8B full fine-tuning
python3 src/run/train.py --config config/example_llama3.1_full.yaml --mode train
```

&nbsp;

&nbsp; -->


## Tutorials & Documentations
1. [Running a FHIR server](./docs/1_fhir_server.md)

&nbsp;

&nbsp;

<!-- ## Bug Reports
If an error occurs while executing the code, check if any of the cases below apply.
* [Bug Cases](./docs/bugs.md) -->


## References 📚
* [FHIR official documents](https://hl7.org/fhir/)
* [Low level FHIR project](https://github.com/hapifhir/hapi-fhir)
* [FHIR Docker server](https://github.com/hapifhir/hapi-fhir-jpaserver-starter)