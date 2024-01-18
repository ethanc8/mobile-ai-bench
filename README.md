# MobileAIBench

## Quickstart (target ARM Linux via SSH)

The MobileAIBench runner must be run on a host system, which connects over SSH to the target on which it will be run.

Although it's theoretically possible to run it on an ARM host system, you would need to get an ARM64 binary of Bazel 0.13, which is hard to find and I wasn't able to build it.

Configure ssh devices in `generic-mobile-devices/devices_for_ai_bench.yml`, for example:
```yaml
devices:
  nanopi:
    target_abis: [aarch64, armhf]
    target_socs: RK3333
    models: Nanopi M4
    address: 10.231.46.118
    username: pi
```

Now, install the dependencies and run the benchmarks.

```bash
conda env update -n MobileAIBench --file environment.yml
conda activate MobileAIBench
# Use your preferred SSH Askpass
# You can save the SSH password in KWallet and have it autofill
# if you use ksshaskpass
SSH_ASKPASS="$(which ksshaskpass)" \
    bash tools/benchmark.sh \
        --benchmark_option=Performance \
        --executors=MACE,TFLITE,NCNN,MNN,TNN \
        --target_abis=aarch64 \
    && python3 report/csv_to_html.py
```