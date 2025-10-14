## MCAL模块依赖

```mermaid
graph LR
A[MCU]-->B[PORT]
B-->C[DIO]
B-->D[ADC]
B-->E[CAN]
B-->F[ETH]
B-->J
A-->G[FLS]-->H[FEE]
A-->I[GPT]-->J[PWM]
A-->K[WDG]

```

