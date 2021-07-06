## 创建一个table环境

```python
t_env = BatchTableEnvironment.create(
  environment_settings=EnvironmentSettings.new_instance().in_batch_mode().use_blink_planner().build())
```

