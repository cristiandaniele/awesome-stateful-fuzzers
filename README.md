# awesome-stateful-fuzzers

This repository contains a list of stateful fuzzers, organised according to the categories presented in the paper "Fuzzers for stateful systems: Survey and Research Directions"

# Categories of fuzzers
- **Grammar Based**: Take in input *a* grammar. It can be the grammar of the messages or the grammar of the traces to produce slightly malformed yet grammar-compliant messages.
- **Grammar Learner**: Similar to grammar-based ones, they automatically infer the state model or the message structure starting from messages or traces.
- **Evolutionary**: Similar to the majority of the stateless fuzzers, they usually take in input the binary file of the software and a sample of messages (often called seed files) to mutate messages that likely trigger bugs.
- **Evolutionary  Grammar-based**: Similar to the Evolutionary ones, they also take grammar as input.
- **Evolutionary Grammar-learner**: Similar to the Evolutionary Grammar-learner ones, they also try to *infer* the state model or the structure of the messages
- **Man in the Middle**: Very different from the above-mentioned ones, they sit between client and server, intercept messages, mutate and forward them.
- **Machine Learning based**: This category also contains fuzzers very different from the first. In fact, it contains fuzzers that rely on ML techniques to produce *ready-to-forward* messages or traces.

# What do the fields mean? 
- **Fuzzer Name**: quite easy
- **Based on**: fuzzer, approach or methodology the *fuzzer* relies on.
- **Mutates**: Which part of the input the *fuzzer* mutates. Options are: **messages**, **traces** or **both**.
- **Input Needed**: Input the *fuzzer* needs in input. Options are: **messages**, **traces**, **PCAP file**, ...
- **Feedback System** (same as **Feedback (i)**): Technique/s the *fuzzer* uses to mutate the messages.
- **Feedback (ii)**: Technique/s the *fuzzer* uses to mutate the message orders.
- **Limitations**: Limitations that characterize the *fuzzer*.

# Grammar Based fuzzers

| Fuzzer Name                                                                | Based on | Mutates          | Open source |
| -------------------------------------------------------------------------- | -------- | ---------------- | ----------- |
| [_AspFuzz_](https://ieeexplore.ieee.org/document/5546704)                  | Na       | Messages & trace | Yes         |
| [_BooFuzz_](https://github.com/jtpereyda/boofuzz)                          | Na       | Messages         | Yes         |
| [_Fuzzowski_](https://github.com/nccgroup/fuzzowski)                       | Sulley   | Messages         | Yes         |
| [_Peach_](https://wiki.mozilla.org/Security/Fuzzing/Peach)                 | Na       | Messages         | Yes         |
| [_PROTOS_](https://link.springer.com/chapter/10.1007/978-0-387-35413-2_16) | Na       | Messages         | No          |
| [_SNOOZE_](https://link.springer.com/chapter/10.1007/11836810_25)          | Na       | Messages         | No          |
| [_Sulley_](https://github.com/OpenRCE/sulley)                              | BooFuzz  | Messages & trace | Yes         |

# Grammar Learner fuzzers

| Fuzzer Name                                                                                          | What it learns                | Based on         | Input needed | Open source |
| ---------------------------------------------------------------------------------------------------- | ----------------------------- | ---------------- | ------------ | ----------- |
| [_Doupé et al._](https://www.usenix.org/system/files/conference/usenixsecurity12/sec12-final225.pdf) | State model                   | Active Learning  | Messages     | Yes         |
| [_GLADE_](https://wcventure.github.io/FuzzingPaper/Paper/Springer15_PULSAR.pdf)                      | Message fields                | Active Learning  | Messages     | Yes         |
| [_Hsu et al._](https://wcventure.github.io/FuzzingPaper/Paper/Springer15_PULSAR.pdf)                 | State models                  | Passive Learning | Traces       | No          |
| [_LearnLib_](https://www.sciencedirect.com/science/article/pii/0890540187900526)                     | Message fields                | Active Learning  | Messages     | Yes         |
| [_Pulsar_](https://dl.acm.org/doi/pdf/10.1145/3140587.3062349)                                       | State models & message fields | Passive Learning | Traces       | Yes         |
| [_StateInspector_](https://dl.acm.org/doi/pdf/10.1145/3548606.3559365)                               | State model                   | Active Learning  | Messages     | Yes         |

# Evolutionary fuzzers

| Fuzzer Name                                                                | Feedback system      | Based On                        | Input needed                                              | Open source |
| -------------------------------------------------------------------------- | -------------------- | ------------------------------- | --------------------------------------------------------- | ----------- |
| [_Chen et al._](https://dl.acm.org/doi/pdf/10.1145/3526088)                | Coverage             | AFL                             | SUT binary, protocol specification, seed files (optional) | Yes         |
| [_FitM fuzzer_](https://www.ndss-symposium.org/ndss-paper/auto-draft-295/) | Coverage             | AFL                             | SUT binary, protocol specification, seed files (optional) | Yes         |
| [_IJON_](https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=9152719) | Coverage & variables | AFL & Manual code annotation    | SUT source code                                           | Yes         |
| [_nyx-net_](https://dl.acm.org/doi/pdf/10.1145/3492321.3519591)            | Coverage             | AFL                             | SUT binary, protocol specification, seed files (optional) | Yes         |
| [_SGFuzz_](https://mboehme.github.io/paper/USENIX22.pdf)                   | Coverage & branches  | AFL & Automatic code annotation | SUT binary, seed files                                    | Yes         |
| [_SNPSfuzzer_](https://arxiv.org/pdf/2202.03643)                           | Coverage & branches  | AFL & Manual code annotation    | SUT binary, seed files                                    | No          |

# Evolutionary grammar-based fuzzers

| Fuzzer Name                                                                        | Learns                       | Feedback (i) | Feedback (ii) | Based on                         | Inputs needed         | Open source |
| ---------------------------------------------------------------------------------- | ---------------------------- | ------------ | ------------- | -------------------------------- | --------------------- | ----------- |
| [_AFLNet_](https://mboehme.github.io/paper/ICST20.AFLNet.pdf)                      | State model                  | Coverage     | Response      | AFL                              | SUT binary, traces    | Yes         |
| [_BLEEM_](https://www.usenix.org/system/files/usenixsecurity23-luo-zhengxiong.pdf) | State model                  | Coverage     | Response      | Packet oriented black-box fuzzer | PCAP file             | Yes         |
| [_FFUZZ_](https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=9565673)        | State model                  | Coverage     | Response      | AFLNet                           | SUT binary, traces    | Yes         |
| [_SGPFuzzer_](https://ieeexplore.ieee.org/document/9200640)                        | State model & Message fields | Coverage     | Response      | AFLNet                           | SUT binary, traces    | Yes         |
| [_StateAFL_](https://link.springer.com/article/10.1007/s10664-022-10233-3)         | State model                  | Coverage     | Memory        | AFL                              | SUT binary, PCAP file | Yes         |


# Man-in-the-Middle Fuzzers

| Fuzzer Name                                                                                                                                                                              | Limitations                                                             | Based on         | Inputs needed | Open source |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------- | ---------------- | ------------- | ----------- |
| [_AutoFuzz_](https://link.springer.com/article/10.1007/s11219-025-09707-6)                                                                                                               | Cannot fuzz the message order                                           | Passive learning | Live traffic  | Yes         |
| [_Black-Box Live Protocol Fuzzing_](https://conference.c3w.at/media/Black_Box_Live_Protocol_Fuzzing.pdf)                                                                                 | Cannot fuzz the message order, user needs to specify the fields to fuzz | Na               | Live traffic  | Yes         |
| [_SecFuzz_](https://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=6228985&casa_token=bpZ0912ma0UAAAAA:uMExaYCo7FQ4nEt9h4SANxlUSpr0Jr7UlDiyB4oWHb5Aj4V4rFih3QyjP85XooTh-_NeiCwoyn1R&tag=1) | Limited fuzzing of message order                                        | Na               | Live traffic  | Yes         |



# Machine Learning Fuzzers
| Fuzzer Name                                                                                                                                                              | Based on      | Inputs needed | Open source |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------- | ------------- | ----------- |
| [_GANFuzz_](https://dl.acm.org/doi/pdf/10.1145/3203217.3203241?casa_token=jUr5t5SZOV8AAAAA:xfjVbpITyW8ZBbeHqvbnhFZXIx9cGbL4WSWVIM9l8bctbaBdH-tx6jDHrAKIamWBB9JqgR39cM5l) | seq-gan model | Traces        | No          |
| [_MachineLearning for Black-box Fuzzing of NetworkProtocols_](https://link.springer.com/chapter/10.1007/978-3-319-89500-0_53)                                            | seq2seqmodel  | Traces        | No          |
| [_SeqFuzzer_](https://wcventure.github.io/FuzzingPaper/Paper/ICST19_SeqFuzzer.pdf)                                                                                       | seq2seqmodel  | Traces        | No          |

# How to contribute

To contribute, you can open a PR with either:

- A fuzzer which is not present in the list.
- A correction. For instance, you think a fuzzer should belong to a different category.
- Ideas to improve the overall structure.
