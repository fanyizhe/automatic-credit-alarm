# 快速创建用户账单警报

## 简介
通过CFN创建3个账单警报以及一个SNS主题，每个警报检测静态的阈值(threshold)。阈值是根据用户输入的credit数乘以不同的系数来定的。警报触发后会发送邮件到这个SNS。

[Quick Start (China Region)](https://console.amazonaws.cn/cloudformation/home?region=cn-north-1#/stacks/new?stackName=billing-alarm&templateURL=https://automatic-credit-alarm.s3.cn-north-1.amazonaws.com.cn/automatic-credit-alarm-cn.yml) 

[Quick Start (Global Region)](https://console.amazonaws.cn/cloudformation/home?region=us-east-1#/stacks/new?stackName=billing-alarm&templateURL=https://automatic-credit-alarm.s3.cn-north-1.amazonaws.com.cn/automatic-credit-alarm-global.yml)



## 前提条件

需要在 “我的账单控制面板 - 账单首选项” 中勾选 "接收账单警报" 来使得CloudWatch能开始监控AWS使用费用。

## 实现步骤

****步骤一：启动资源****

- 在您的AWS账户中启动 AWS CloudFormation 模板。

- 检查导航栏右上角显示的所在区域，根据需要进行更改。

        注意：Global地区的账户只能在弗吉尼亚北部(us-east-1)创建账单警报。

- 在 **指定模板** 页面上，保留模板 URL 的默认设置，然后选择 **下一步** 。

- 在 **指定堆栈详细信息** 页面上，输入堆栈名称，以及设定的参数，参数说明如下：

    - credit: 您希望设置为阈值的信用额度
    - emailEndPoint: 您用以接受警报邮件的邮箱
    - thresholdPercentage: 您希望设置的阈值百分比（e.g. 70%的最大信用额度），输入一个0-1之间的小数。需要输入3个值(e.g. 0.5, 0.7, 1)。

- （可选）在 **配置堆栈选项** 页面上，您可以为堆栈中的资源 [指定标签](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-resource-tags.html) (键/值对) 并 [设置高级选项](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-console-add-tags.html)。在完成此操作后，选择 **下一步** 。

- 在 **审核** 页面上，查看并确认模板设置。选择 **功能** 下的复选框，以确认模板将创建 IAM 资源。

- 选择 **创建堆栈** 以部署堆栈。
- 监控堆栈的状态。当状态为 **CREATE\_COMPLETE** 时，表示脚本及资源创建已完成。



****步骤二： 确认订阅****

在您完成资源创建后，您会收到一封确认订阅的邮件。您需要点击 **Comfirm Subscription** 以确认订阅该事件。





## 限制

- 账单警报的监测频率是每6小时检测一次，该频率无法修改。
- CloudWatch不支持在一个警报中监测多个值，或者设置多个警报阈值。所以目前使用创建多个警报的方法来设置不同的警报阈值。
- 目前代表该额度上限的阈值不会每个月自动变化，而每个月检测的总花费会清零，这需要后续的开发。
- 用户暂时无法在不更改模板代码的情况下自定义想要创建警报的数量(目前固定3个)
- 非中国区的用户只能在弗吉尼亚北部(us-east-1)创建账单警报。

