### 主要训练

主要训练入口在sam3/sam3/train/train.py，根据官方文档Evaluation评估也可以用train.py文件，只需设置`trainer.mode = val`就可以实现评估

如 
```bash
# Example: Evaluate on Roboflow dataset
python sam3/train/train.py -c configs/roboflow_v100/roboflow_v100_eval.yaml
```

Transformations 图片变换  sam3/train/transforms/

构建模型 sam3.sam3.model_builder.build_sam3_image_model，设置checkpoint_path，可在build_sam3_image_model时加载checkpoint. 注意即使用不到bpe也要加载，bpe_path是必填

构建模型处理器 sam3.sam3.model.sam3_image_processor.Sam3Processor，加载设置刚才构建的模型model，并设置置信度(比如0.5)


inference_state = processor.set_image(img)
inference_state让处理器处理图片并产出结果


processor.reset_all_prompts(inference_state)
inference_state = processor.add_geometric_prompt(
    state=inference_state, box=norm_box_cxcywh, label=True
)

重置prompts并重新设置prompt,可以设置单图或组图，也可以设置文字prompt或bbox prompt


plot_results(img0, inference_state)
利用官方visualize.py显示inference results



eval主要在sam3.sam3.eval里面有各种eval file
collator在sam3.sam3.train.data.collator.py（但是不确定是否有用）
