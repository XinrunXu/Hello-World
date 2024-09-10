[toc]
# 复现他人代码中，解决不了的问题
# Seq2Act
版本要求为 tensorflow==1.15，但是跟它的代码很多地方不适配
train 的时候报错:
``` python
ValueError: An operation has None for gradient. Please make sure that all of your ops have a gradient defined (i.e. are differentiable). Common ops without gradient: K.argmax, K.round, K.eval.
```
eval  的时候报错:
``` python
Traceback (most recent call last):
  File "...google-research/lib/python3.7/site-packages/tensorflow/python/training/saver.py", line 1426, in restore
    names_to_keys = object_graph_key_mapping(save_path)
  File "...google-research/lib/python3.7/site-packages/tensorflow/python/training/saver.py", line 1748, in object_graph_key_mapping
    object_graph_string = reader.get_tensor(trackable.OBJECT_GRAPH_PROTO_KEY)
  File "...google-research/lib/python3.7/site-packages/tensorflow/python/training/py_checkpoint_reader.py", line 71, in get_tensor
    error_translator(e)
  File "...google-research/lib/python3.7/site-packages/tensorflow/python/training/py_checkpoint_reader.py", line 31, in error_translator
    raise errors_impl.NotFoundError(None, None, error_message)
tensorflow.python.framework.errors_impl.NotFoundError: Key _CHECKPOINTABLE_OBJECT_GRAPH not found in checkpoint

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "/root/miniconda3/envs/seq2act/lib/python3.7/runpy.py", line 193, in _run_module_as_main
    "__main__", mod_spec)
  File "/root/miniconda3/envs/seq2act/lib/python3.7/runpy.py", line 85, in _run_code
    exec(code, run_globals)
  File "...google-research/seq2act/bin/seq2act_decode.py", line 219, in <module>
    tf.app.run()
  File "...google-research/lib/python3.7/site-packages/tensorflow/python/platform/app.py", line 36, in run
    _run(main=main, argv=argv, flags_parser=_parse_flags_tolerate_undef)
  File "...google-research/lib/python3.7/site-packages/absl/app.py", line 308, in run
    _run_main(main, args)
  File "...google-research/lib/python3.7/site-packages/absl/app.py", line 254, in _run_main
    sys.exit(main(argv))
  File "...google-research/seq2act/bin/seq2act_decode.py", line 216, in main
    decode_fn(hparams)
  File "...google-research/seq2act/bin/seq2act_decode.py", line 165, in decode_fn
    saver.restore(session, latest_checkpoint)
  File "...google-research/lib/python3.7/site-packages/tensorflow/python/training/saver.py", line 1432, in restore
    err, "a Variable name or other graph key that is missing")
tensorflow.python.framework.errors_impl.NotFoundError: Restoring from checkpoint failed. This is most likely due to a Variable name or other graph key that is missing from the checkpoint. Please ensure that you have not altered the graph expected based on the checkpoint. Original error:

Graph execution error:

Detected at node 'save/RestoreV2' defined at (most recent call last):
    File "/root/miniconda3/envs/seq2act/lib/python3.7/runpy.py", line 193, in _run_module_as_main
      "__main__", mod_spec)
    File "/root/miniconda3/envs/seq2act/lib/python3.7/runpy.py", line 85, in _run_code
      exec(code, run_globals)
    File "...google-research/seq2act/bin/seq2act_decode.py", line 219, in <module>
      tf.app.run()
    File "...google-research/lib/python3.7/site-packages/absl/app.py", line 308, in run
      _run_main(main, args)
    File "...google-research/lib/python3.7/site-packages/absl/app.py", line 254, in _run_main
      sys.exit(main(argv))
    File "...google-research/seq2act/bin/seq2act_decode.py", line 216, in main
      decode_fn(hparams)
    File "...google-research/seq2act/bin/seq2act_decode.py", line 159, in decode_fn
      saver = tf.train.Saver()
Node: 'save/RestoreV2'
Key compute_grounding_logits_1/encode_screen/encoder/layer_0/ffn/conv1/bias not found in checkpoint
         [[{{node save/RestoreV2}}]]

Original stack trace for 'save/RestoreV2':
  File "/root/miniconda3/envs/seq2act/lib/python3.7/runpy.py", line 193, in _run_module_as_main
    "__main__", mod_spec)
  File "/root/miniconda3/envs/seq2act/lib/python3.7/runpy.py", line 85, in _run_code
    exec(code, run_globals)
  File "...google-research/seq2act/bin/seq2act_decode.py", line 219, in <module>
    tf.app.run()
  File "...google-research/lib/python3.7/site-packages/tensorflow/python/platform/app.py", line 36, in run
    _run(main=main, argv=argv, flags_parser=_parse_flags_tolerate_undef)
  File "...google-research/lib/python3.7/site-packages/absl/app.py", line 308, in run
    _run_main(main, args)
  File "...google-research/lib/python3.7/site-packages/absl/app.py", line 254, in _run_main
    sys.exit(main(argv))
  File "...google-research/seq2act/bin/seq2act_decode.py", line 216, in main
    decode_fn(hparams)
  File "...google-research/seq2act/bin/seq2act_decode.py", line 159, in decode_fn
    saver = tf.train.Saver()
  File "...google-research/lib/python3.7/site-packages/tensorflow/python/training/saver.py", line 931, in __init__
    self.build()
  File "...google-research/lib/python3.7/site-packages/tensorflow/python/training/saver.py", line 943, in build
    self._build(self._filename, build_save=True, build_restore=True)
  File "...google-research/lib/python3.7/site-packages/tensorflow/python/training/saver.py", line 981, in _build
    build_restore=build_restore)
  File "...google-research/lib/python3.7/site-packages/tensorflow/python/training/saver.py", line 541, in _build_internal
    restore_sequentially, reshape)
  File "...google-research/lib/python3.7/site-packages/tensorflow/python/training/saver.py", line 361, in _AddRestoreOps
    restore_sequentially)
  File "...google-research/lib/python3.7/site-packages/tensorflow/python/training/saver.py", line 608, in bulk_restore
    return io_ops.restore_v2(filename_tensor, names, slices, dtypes)
  File "...google-research/lib/python3.7/site-packages/tensorflow/python/ops/gen_io_ops.py", line 1520, in restore_v2
    name=name)
  File "...google-research/lib/python3.7/site-packages/tensorflow/python/framework/op_def_library.py", line 797, in _apply_op_helper
    attrs=attr_protos, op_def=op_def)
  File "...google-research/lib/python3.7/site-packages/tensorflow/python/framework/ops.py", line 3806, in _create_op_internal
    op_def=op_def)
```

# UIBert
1. 代码中公开的数据集和文章中描述的数据集数量完全对不上
2. 没有公开任何除了数据集之外的代码，甚至不知道怎么用同样的数据集在其他模型上测试
