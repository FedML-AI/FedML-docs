# FedML APIs (core) Introduction
FedML-core separates the communication and the model training into two core components. 
The first is the communication protocol component (labeled as distributed in the figure). 
It is responsible for low-level communication among different works in the network. The communication backend is based on MPI (message passing interface, https://pypi.org/project/mpi4py/). 
We consider adding more backends as necessary, such as RPC (remote procedure call). 
Inside the communication protocol component, a TopologyManager supports the flexible topology configuration required by different distributed learning algorithms. 
The second is the on-device deep learning component, which is built based on the popular deep learning framework PyTorch or TensorFlow. 
For flexibility, there is no restriction on the framework for this part. 
Users can implement trainers and coordinators according to their needs. 
In addition, low-level APIs support security and privacy-related algorithms.

The philosophy of the FedML programming interface is to provide the simplest user experience: 
allowing users to build distributed training applications; 
to design customized message flow and topology definitions 
by only focusing on algorithmic implementations while ignoring the low-level communication backend details.

## Worker-Oriented Programming
FedML-core provides the worker-oriented programming design pattern, which can be used to program the worker behavior 
when participating in training or coordination in the FL algorithm. 
We describe it as worker-oriented because its counterpart, the standard distributed training library 
(e.g., [torch.distributed](https://pytorch.org/tutorials/intermediate/dist_tuto.html)), 
normally completes distributed training programming by describing the entire training procedure rather than focusing on the behavior of each worker. 

With the worker-oriented programming design pattern, the user can customize its own worker in FL network 
by inheriting the WorkerManager class and utilizing its predefined APIs register_message_receive_handler and send_message to 
define the receiving and sending messages without considering the underlying communication mechanism (as shown in the highlight blue box
in Figure 1). 
Conversely, existing distributed training frameworks do not have such flexibility for algorithm innovation. 
In order to make the comparison clearer, we use the most popular machine learning framework PyTorch as an example. 
Figure 1 illustrates a complete training procedure (distributed synchronous SGD) 
and aggregates gradients from all other workers with the all_reduce messaging passing interface. 
Although it supports multiprocessing training, it cannot flexibly customize different messaging flows in any network topology. 
In PyTorch, another distributed training API, [torch.nn.parallel.paraDistributedDataParallel](https://pytorch.org/tutorials/intermediate/ddp_tutorial.html) also has such inflexibly. 
Note that [torch.distributed.rpc](https://pytorch.org/tutorials/intermediate/rpc\_tutorial.html) is a low-level communication back API 
that can finish any communication theoretically, but it is not user-friendly for federated learning researchers. 

## Message Definition Beyond Gradient and Model
FedML also considers supporting message exchange beyond the gradient or model from the perspective of message flow. 
This type of auxiliary information may be due to either the need for algorithm design or the need for system-wide configuration delivery. 
Each worker defines the message type from the perspective of sending. 
Thus, in the above introduced worker-oriented programming, WorkerManager should handle messages defined by other trainers and also send messages defined by itself. 
The sending message is normally executed after handling the received message. As shown in Figure 2, in the yellow background highlighted code snippet, 
workers can send any message type and related message parameters during the train() function.

## Topology management
As demonstrated in Figure 3, FL has various topology definitions, 
such as vertical FL, split learning, decentralized FL, and Hierarchical FL. 
In order to meet such diverse requirements, FedML provides TopologyManager to manage the topology 
and allow users to send messages to arbitrary neighbors during training. 
Specifically, after the initial setting of TopologyManager is completed, 
for each trainer in the network, the neighborhood worker ID can be queried through the TopologyManager. 
In line 26 of Figure 2, we see that the trainer can query its neighbor nodes 
through the TopologyManager before sending its message. 

## Trainer and coordinator
We also need the coordinator to complete the training (\textit{e.g.}, in the FedAvg algorithm, the central worker is the coordinator while the others are trainers). For the trainer and coordinator, \texttt{FedML} does not over-design. Rather, it gives the implementation completely to the developers, reflecting the flexibility of our framework. The implementation of the trainer and coordinator is similar to the process in Figure \ref{fig:overview_training_oriented}, which is completely consistent with the training implementation of a standalone version training. We provide some reference implementations of different trainers and coordinators in our source code (Section \ref{sec:reference_examples}).


## Privacy, security, and robustness
While the FL framework facilitates data privacy \cite{mirshghallah2020privacy} by keeping data locally available to the users and only requiring communication for model updates, users may still be concerned about partial leakage of their data which may be inferred from the communicated model (see, \textit{e.g.},~\cite{fredrikson2015model}). Aside from protecting the privacy of users' data, another critical security requirement for the FL platform, especially when operating over mobile devices, is the robustness towards user dropouts. Specifically, to accomplish the aforementioned goals of achieving security, privacy, and robustness for FL, various cryptography and coding-theoretic approaches have been proposed to manipulate intermediate model data~(see \cite{bonawitz2017practical,so2020turbo}).

To facilitate rapid implementation and evaluation of data manipulation techniques to enhance security, privacy, and robustness, we include low-level APIs that implement common cryptographic primitives such as secrete sharing, key agreement, digital signature, and public key infrastructure. We also plan to include an implementation of \texttt{Lagrange Coded Computing} (LCC)~\cite{yu2019lagrange}. LCC is a recently developed coding technique on data that achieves optimal resiliency, security (against adversarial nodes), and privacy for any polynomial evaluations on the data. Finally, we plan to provide a sample implementation of the secure aggregation algorithm~\cite{bonawitz2017practical}, using the above APIs.

In standard FL settings, it is assumed that there is no single central authority that owns or verifies the training data or user hardware, and it has been argued by many recent studies that FL lends itself to new adversarial attacks during decentralized model training~\cite{bagdasaryan2020backdoor,sun2019can,bhagoji2019analyzing,xie2019dba,wang2020attack}. Several robust aggregation methods have been proposed to enhance the robustness of FL against adversaries ~\cite{sun2019can,pillutla2019robust,blanchard2017machine}.

To accelerate generating benchmark results on new types of adversarial attacks in FL, we include the latest robust aggregation methods presented in literature \textit{i.e.} (i) norm difference clipping~\cite{sun2019can}; weak differential private (DP)~\cite{sun2019can}; (ii) RFA (geometric median)~\cite{pillutla2019robust}; (iii) \textsc{Krum} and (iv) \textsc{Multi-Krum}~\cite{blanchard2017machine}. Our APIs are easily extendable to support newly developed types of robust aggregation methods. On the attack end, we observe that most of the existing attacks are highly task-specific. Thus, it is challenging to provide general adversarial attack APIs. Our APIs support the \textit{backdoor with model replacement attack} presented in~\cite{bagdasaryan2020backdoor} and the \textit{edge-case backdoor attack} presented in~\cite{wang2020attack} to provide a reference for researchers to develop new attacks.