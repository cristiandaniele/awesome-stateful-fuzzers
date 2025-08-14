# awesome-stateful-fuzzers

This repository contains a list of stateful fuzzers, organised according to the categories presented in the survey paper ["Fuzzers for stateful systems: Survey and Research Directions"](https://dl.acm.org/doi/abs/10.1145/3648468) by
Cristian Daniele, Seyed Benham Andarzian and Erik Poll
that appeared in ACM Computing Surveys in April 2024.

Since the publication of that survey several new stateful fuzzers have been appeared. To keep track of them we started this github page. In fact, there have also been more survey papers about stateful fuzzing (aka protocol fuzzing or stateful protocol fuzzing); we also list these below.

## Short background on fuzzing

**Fuzzing** is a testing technique which has been proven very effective in finding vulnerabilities in software. In a nutshell, a fuzzer sends millions of slightly malformed messages to an SUT (System Under Test) hoping to detect crashes which signal vulnerabilities, typically memory corruption flaws.
Fuzzers use various tricks (grammars, seed files, code-coverage-guided evolution, ...) to cleverly generate messages to feed to the SUT, with the goal to maximise code coverage and find as many bugs as possible.

Most fuzzers target *stateless* systems. Here the processing of an input message does not depend on previous inputs. Typical examples are graphics libraries for viewing or converting images.

However, many systems, for instance protocol implementations, are *stateful*. Here the system keep an internal state to be able to properly process *sequences of messages* aka *traces*.  For example, an FTP server needs to remember that the user *Cris* already inserted the correct password. The internal state recorded by the SUT which then influences the way in which future messages are processed complicates the job for the fuzzer.  To fuzz a stateless system the fuzzer only has to mutate individual messages, but to fuzz a stateful system the fuzzer needs to mutate not just messages but also the order of messages in traces.

Different fuzzers use different approaches to deal with the statefulness of the systems, as explained in our paper and summarised in this repo.

## Relation between stateful fuzzing and active learning (aka active automata learning)

As already mentioned, fuzzers for stateful systems need to keep into account the stateful nature of the SUT. One way of inferring this state behaviour is by using active learning tools.

Very briefly, we can picture active learning tools as fuzzers which send messages to the SUT and observe the responses in order to infer an approximation of the state model of the SUT. If you want to know more or play around with active learning tools you can visit [the Automata Wiki](https://automata.cs.ru.nl) by [Frits Vaandrager](https://www.cs.ru.nl/~fvaan/) and colleagues. Also, you can find some simple examples of state model learning for FTP servers [here](https://github.com/cristiandaniele/ftp-statemodel-learner).


## Categories of fuzzers from our survey paper
- **Grammar Based**: These require some grammar as input. This can be the grammar for the messages or the grammar of the traces, or both. The grammar for the traces, i.e. the message sequences, is often thought of as a finite state machine. This grammar is used as basis to produce (slightly) malformed messages and message sequences.
- **Grammar Learner**: Instead of requiring a grammar as input, these automatically infer a grammar for the message structure or a grammer (aka state model) for the message sequences or the message structure starting from messages or traces.
- **Evolutionary**: In the evolutionary style popularised by afl, these fuzzers observe execution paths to discover interesting mutations of the initial set of messages (often called seed files).
- **Evolutionary  Grammar-based**: These also take the evolutionary approach, like the Evolutionary fuzzers, but take a grammar as input.
- **Evolutionary Grammar-learner**: These take a similar approach as  Evolutionary Grammar-based fuzzere, but they try to *infer* the state model or the structure of the messages.
- **Man in the Middle**: Very different from the above-mentioned ones, they sit between client and server, intercept messages, mutate and forward them.
- **Machine Learning based**: This category contains fuzzers that rely on ML techniques to produce *ready-to-forward* messages or traces.

## What do the fields mean? 
- **Fuzzer Name**: quite easy
- **Based on**: fuzzer, approach or methodology the *fuzzer* relies on.
- **Mutates**: Which part of the input the *fuzzer* mutates. Options are: **messages**, **traces** or **both**.
- **Input Needed**: Input the *fuzzer* needs in input. Options are: **messages**, **traces**, **PCAP file**, ...
- **Feedback System** (same as **Feedback (i)**): Technique/s the *fuzzer* uses to mutate the messages.
- **Feedback (ii)**: Technique/s the *fuzzer* uses to mutate the message orders.
- **Limitations**: Limitations that characterize the *fuzzer*.





## Grammar Based fuzzers

| Fuzzer Name                                                                | Based on | Mutates          | Open source |
| -------------------------------------------------------------------------- | -------- | ---------------- | ----------- |
| [_AspFuzz_](https://ieeexplore.ieee.org/document/5546704)                  | NA       | Messages & trace | Yes         |
| [_BooFuzz_](https://github.com/jtpereyda/boofuzz)                          | NA       | Messages         | Yes         |
| [_Fuzzowski_](https://github.com/nccgroup/fuzzowski)                       | Sulley   | Messages         | Yes         |
| [_Peach_](https://wiki.mozilla.org/Security/Fuzzing/Peach)                 | NA       | Messages         | Yes         |
| [_PROTOS_](https://link.springer.com/chapter/10.1007/978-0-387-35413-2_16) | NA       | Messages         | No          |
| [_SNOOZE_](https://link.springer.com/chapter/10.1007/11836810_25)          | NA       | Messages         | No          |
| [_Sulley_](https://github.com/OpenRCE/sulley)                              | BooFuzz  | Messages & trace | Yes         |

## Grammar Learner fuzzers

| Fuzzer Name                                                                                          | What it learns                | Based on         | Input needed | Open source |
| ---------------------------------------------------------------------------------------------------- | ----------------------------- | ---------------- | ------------ | ----------- |
| [_Doupé et al._](https://www.usenix.org/system/files/conference/usenixsecurity12/sec12-final225.pdf) | State model                   | Active Learning  | Messages     | Yes         |
| [_GLADE_](https://wcventure.github.io/FuzzingPaper/Paper/Springer15_PULSAR.pdf)                      | Message fields                | Active Learning  | Messages     | Yes         |
| [_Hsu et al._](https://wcventure.github.io/FuzzingPaper/Paper/Springer15_PULSAR.pdf)                 | State models                  | Passive Learning | Traces       | No          |
| [_LearnLib_](https://www.sciencedirect.com/science/article/pii/0890540187900526)                     | Message fields                | Active Learning  | Messages     | Yes         |
| [_Pulsar_](https://dl.acm.org/doi/pdf/10.1145/3140587.3062349)                                       | State models & message fields | Passive Learning | Traces       | Yes         |
| [_StateInspector_](https://dl.acm.org/doi/pdf/10.1145/3548606.3559365)                               | State model                   | Active Learning  | Messages     | Yes         |

## Evolutionary fuzzers

| Fuzzer Name                                                                | Feedback system      | Based On                        | Input needed                                              | Open source |
| -------------------------------------------------------------------------- | -------------------- | ------------------------------- | --------------------------------------------------------- | ----------- |
| [_Chen et al._](https://dl.acm.org/doi/pdf/10.1145/3526088)                | Coverage             | AFL                             | SUT binary, protocol specification, seed files (optional) | Yes         |
| [_FitM fuzzer_](https://www.ndss-symposium.org/ndss-paper/auto-draft-295/) | Coverage             | AFL                             | SUT binary, protocol specification, seed files (optional) | Yes         |
| [_IJON_](https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=9152719) | Coverage & variables | AFL & Manual code annotation    | SUT source code                                           | Yes         |
| [_nyx-net_](https://dl.acm.org/doi/pdf/10.1145/3492321.3519591)            | Coverage             | AFL                             | SUT binary, protocol specification, seed files (optional) | Yes         |
| [_SGFuzz_](https://mboehme.github.io/paper/USENIX22.pdf)                   | Coverage & branches  | AFL & Automatic code annotation | SUT binary, seed files                                    | Yes         |
| [_SNPSfuzzer_](https://arxiv.org/pdf/2202.03643)                           | Coverage & branches  | AFL & Manual code annotation    | SUT binary, seed files                                    | No          |

## Evolutionary grammar-based fuzzers

| Fuzzer Name                                                                        | Learns                       | Feedback (i) | Feedback (ii) | Based on                         | Inputs needed         | Open source |
| ---------------------------------------------------------------------------------- | ---------------------------- | ------------ | ------------- | -------------------------------- | --------------------- | ----------- |
| [_AFLNet_](https://mboehme.github.io/paper/ICST20.AFLNet.pdf)                      | State model                  | Coverage     | Response      | AFL                              | SUT binary, traces    | Yes         |
| [_BLEEM_](https://www.usenix.org/system/files/usenixsecurity23-luo-zhengxiong.pdf) | State model                  | Coverage     | Response      | Packet oriented black-box fuzzer | PCAP file             | Yes         |
| [_FFUZZ_](https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=9565673)        | State model                  | Coverage     | Response      | AFLNet                           | SUT binary, traces    | Yes         |
| [_SGPFuzzer_](https://ieeexplore.ieee.org/document/9200640)                        | State model & Message fields | Coverage     | Response      | AFLNet                           | SUT binary, traces    | Yes         |
| [_StateAFL_](https://link.springer.com/article/10.1007/s10664-022-10233-3)         | State model                  | Coverage     | Memory        | AFL                              | SUT binary, PCAP file | Yes         |


## Man-in-the-Middle Fuzzers

| Fuzzer Name                                                                                                                                                                              | Limitations                                                             | Based on         | Inputs needed | Open source |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------- | ---------------- | ------------- | ----------- |
| [_AutoFuzz_](https://link.springer.com/article/10.1007/s11219-025-09707-6)                                                                                                               | Cannot fuzz the message order                                           | Passive learning | Live traffic  | Yes         |
| [_Black-Box Live Protocol Fuzzing_](https://conference.c3w.at/media/Black_Box_Live_Protocol_Fuzzing.pdf)                                                                                 | Cannot fuzz the message order, user needs to specify the fields to fuzz | NA               | Live traffic  | Yes         |
| [_SecFuzz_](https://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=6228985&casa_token=bpZ0912ma0UAAAAA:uMExaYCo7FQ4nEt9h4SANxlUSpr0Jr7UlDiyB4oWHb5Aj4V4rFih3QyjP85XooTh-_NeiCwoyn1R&tag=1) | Limited fuzzing of message order                                        | NA               | Live traffic  | Yes         |



## Machine Learning Fuzzers
| Fuzzer Name                                                                                                                                                              | Based on      | Inputs needed | Open source |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------- | ------------- | ----------- |
| [_GANFuzz_](https://dl.acm.org/doi/pdf/10.1145/3203217.3203241?casa_token=jUr5t5SZOV8AAAAA:xfjVbpITyW8ZBbeHqvbnhFZXIx9cGbL4WSWVIM9l8bctbaBdH-tx6jDHrAKIamWBB9JqgR39cM5l) | seq-gan model | Traces        | No          |
| [_MachineLearning for Black-box Fuzzing of NetworkProtocols_](https://link.springer.com/chapter/10.1007/978-3-319-89500-0_53)                                            | seq2seqmodel  | Traces        | No          |
| [_SeqFuzzer_](https://wcventure.github.io/FuzzingPaper/Paper/ICST19_SeqFuzzer.pdf)                                                                                       | seq2seqmodel  | Traces        | No          |


## Recent surveys about stateful fuzzing

Several survey papers about stateful fuzzing that have appeared since 

- ["Fuzzers for stateful systems: Survey and Research Directions"](https://dl.acm.org/doi/abs/10.1145/3648468) by
Cristian Daniele et al., ACM Computing Surveys, Vol. 56, No. 9, 2024

namely

- [A Survey of Protocol Fuzzing](https://dl.acm.org/doi/10.1145/3696788), Xiaohan Zhang et al., ACM Computing Surveys, Vol. 57, No. 2, 2024

- [Fuzzing for Stateful Protocol Implementations: Are We There Yet?](https://link.springer.com/chapter/10.1007/978-3-031-64626-3_11), Kunpeng Jian et al., TASE 2024

- [A Survey of Network Protocol Fuzzing: Model, Techniques and Directions](https://arxiv.org/abs/2402.17394), Shihao Jiang at al., arXiv, 2024

## Recent papers about stateful fuzzers

## How to contribute

To contribute, you can open a PR with either:

- A fuzzer which is not present in the list.
- A correction. For instance, you think a fuzzer should belong to a different category.
- Ideas to improve the overall structure.
