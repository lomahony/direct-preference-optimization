Run Training: 
* Log in wandb 
* Alter config
* train sft model: 
```console
$ python -u train.py model.fsdp_policy_mp=bfloat16 
```
* train dpo model: 
```console
$ python -u train.py loss.beta=0.1 model.archive=.cache/laura/pythia12_hh_ga2_sft_policy_dtype_bfloat16_2023-08-03_13-06-56_410701/LATEST/policy.pt 
```

* Upload model to HF
Log in. 
```console
$ python upload.py ~/hf/eleuther-pythia28-hh-sft_old lomahony/eleuther-pythia2.8b-hh-sft main
```
* evaluate with lm-evaluation-harness
```console
~/lm-evaluation-harness$ python main.py --model hf --model_args pretrained=lomahony/eleuther-pythia2.8b-hh-sft --tasks lambada_openai,hellaswag,arc_easy,arc_challenge,wikitext,winogrande,piqa,boolq,openbookqa,sciq --num_fewshot 0 --batch_size 16 --output_path ~/direct-preference-optimization/model_eval_files/sft-pythia-2.8b-0shot > ~/direct-preference-optimization/model_eval_files/sft-pythia-2.8b-0shot-shelloutput.txt 
```

```console
accelerate launch main.py --model hf --model_args pretrained=lomahony/eleuther-pythia2.8b-hh-sft --tasks lambada_openai,hellaswag,arc_easy,arc_challenge,wikitext,winogrande,piqa,boolq,openbookqa,sciq --num_fewshot 0 --batch_size 16 --output_path ~/direct-preference-optimization/model_eval_files/sft-pythia-2.8b-0shot > ~/direct-preference-optimization/model_eval_files/sft-pythia-2.8b-0shot-shelloutput.txt 
```

#ran in ultrafast:
```console
python main.py --model hf --model_args pretrained=lomahony/eleuther-pythia2.8b-hh-dpo,parallelize=True --tasks lambada_openai,hellaswag,arc_easy,arc_challenge,wikitext,winogrande,piqa,boolq,openbookqa,sciq --num_fewshot 5 --device cuda:1 --batch_size 8 --output_path /home/laura/direct-preference-optimization/model_eval_files/dpo-pythia-2.8b-5shot
```

* Remember to use tmux: 
```console
$ tmux 
```
* Conda environment  direct-preference-optimization
```console
$ conda activate direct-preference-optimization
```

TODO: 
* 12B DPO model 
    - [ ] Fix error
    - [ ] Train model
* lm-evaluation-harness:
    - [ ] evaluate 12, 12-sft, 12-dpo
* GPT-4 eval
    - [ ] Test
    - [ ] Set up 
    - [ ] Evaluate models
* Stats on the Pile 
    - [ ] Perplexity
    - [ ] Entropy