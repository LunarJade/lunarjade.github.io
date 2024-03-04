---
layout: post
title: UVM Questions
---
## 仿真在UVM_ERROR达到一定数量后结束
- 添加run option：`+UVM_MAX_QUIT_COUNT=6,NO`
- 其中第一个参数表示退出阈值，第二个参数NO表示不可愚蠢被后面的语句重载，还可以是YES

## UVM Facttory Override
- 有以下方法
    1. 在tb中
        ```
        initial begin
            factory.set_type_override_by_type(req_type::get_type(), override_type::get_type());
        end
        ```
    2. 在run option中
        ```
        <sim command> +uvm_set_type_override=<req_type>,<override_type>
        ```
- 注意 
    - 在实例化时，UVM会通过factory机制查看是否有重载记录，当有重载记录，会使用新的类型来代替旧的类型，因此Override的类需要实例化才能生效
    - 无论是重载的类还是被重载的类，都要注册到factory中
    - 被重载的类的实例化方式需要使用type_id::create的方式，不能使用new的方式

## run_phase和main_phase
- main_phase是run_phase的12个子phase的其中一个, the main_phase is embeded in the run_phase
- run_phase和其子phase并行执行
- run_phase一定是从0时刻开始，main_phase不一定
- 如果main_phase没有raise_objection，main_phase不会执行；如果main_phase有raise_objection，同时run_phase没有raise_objection，run_phase仍然会执行
