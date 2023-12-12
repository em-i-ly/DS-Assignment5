Assignment 5 
---------------------

# Team Members

- Emily Gallacher

# GitHub link to your (forked) repository
https://github.com/em-i-ly/DS-Assignment5.git


# Task 1

Note: Some questions require you to take screenshots. In that case, please join the screenshots and indicate in your answer which image refer to which screenshot.

1. What happens when Raft starts? Why?

    Ans: 
    The leader election process in the Raft consensus algorithm is initiated by followers when they do not receive heartbeats 
    within a specified timeout. After the timeout, a follower transitions to the candidate state, increments its term, and requests votes 
    from other nodes in the cluster. Nodes vote if they have not voted for another candidate in the current term and if  
    the candidate's log is up-to-date. If a candidate receives votes from a majority of nodes, it becomes the leader for the 
    current term, initiating regular heartbeats and log replication to maintain leadership. 


2. Perform one request on the leader, wait until the leader is committed by all servers. Pause the simulation.
Then perform a new request on the leader. Take a screenshot, stop the leader and then resume the simulation.
Once, there is a new leader, perform a new request and then resume the previous leader. Once, this new request is committed 
by all servers, pause the simulation and take a screenshot. Explain what happened?

    Ans: 
    When following the steps we first send a request to the Server 1 (leader), then we wait for the followers to commit to the leader. 
    While sending a new request to Server 1 we pause the simulation. By doing so we simulate a failure, which will lead to a
    reelection happening. When restarting the simulation the reelection starts and the new leader (Server 5) will handle the second request.
    Now the Server 1 will act in a follower state. 


3. Stop the current leader and two other servers. After a few increase in the Raft term, pause the simulation and take a screenshot. 
Then resume all servers and restart the simulation. After the leader election, pause the simulation and take a screenshot. 
Explain what happened.

    Ans: 
    As I paused the leader, along with two followers, a re-election was attempted. This re-election kept going in a loop because 
    the candidate could never reach a majority vote, as it would need 3 while only 2 nodes were active. Once the simulation was resumed,
    once of the two candidates is elected because it has a higher term than the previous leader. The term of the paused nodes is updated
    to the current one.



# Task 2

1. Indicate the replies that you get from the "/admin/status" endpoint of the HTTP service for each servers. Which server is the leader? Can there be multiple leaders?
    
    Ans:
    Server 0: {'version': '0.3.12', 'revision': 'deprecated', 'self': TCPNode('127.0.0.1:6000'), 'state': 2, 'leader': TCPNode('127.0.0.1:6000'), 'has_quorum': True, 'partner_nodes_count': 2, 'partner_node_status_server_127.0.0.1:6001': 2, 'partner_node_status_server_127.0.0.1:6002': 2, 'readonly_nodes_count': 0, 'log_len': 2, 'last_applied': 2, 'commit_idx': 2, 'raft_term': 1, 'next_node_idx_count': 2, 'next_node_idx_server_127.0.0.1:6001': 3, 'next_node_idx_server_127.0.0.1:6002': 3, 'match_idx_count': 2, 'match_idx_server_127.0.0.1:6001': 2, 'match_idx_server_127.0.0.1:6002': 2, 'leader_commit_idx': 2, 'uptime': 36, 'self_code_version': 0, 'enabled_code_version': 0}
        
    Server 1: {'version': '0.3.12', 'revision': 'deprecated', 'self': TCPNode('127.0.0.1:6001'), 'state': 0, 'leader': TCPNode('127.0.0.1:6000'), 'has_quorum': True, 'partner_nodes_count': 2, 'partner_node_status_server_127.0.0.1:6002': 2, 'partner_node_status_server_127.0.0.1:6000': 2, 'readonly_nodes_count': 0, 'log_len': 2, 'last_applied': 2, 'commit_idx': 2, 'raft_term': 1, 'next_node_idx_count': 0, 'match_idx_count': 0, 'leader_commit_idx': 2, 'uptime': 102, 'self_code_version': 0, 'enabled_code_version': 0}
        
    Server 2: {'version': '0.3.12', 'revision': 'deprecated', 'self': TCPNode('127.0.0.1:6002'), 'state': 0, 'leader': TCPNode('127.0.0.1:6000'), 'has_quorum': True, 'partner_nodes_count': 2, 'partner_node_status_server_127.0.0.1:6001': 2, 'partner_node_status_server_127.0.0.1:6000': 2, 'readonly_nodes_count': 0, 'log_len': 2, 'last_applied': 2, 'commit_idx': 2, 'raft_term': 1, 'next_node_idx_count': 0, 'match_idx_count': 0, 'leader_commit_idx': 2, 'uptime': 116, 'self_code_version': 0, 'enabled_code_version': 0}
        
    Server 0 is the leader. This can be observed with the 'leader' keyword on each server, which in my case each indicate that the TCPNode('127.0.0.1:6000') is the leader. Additionally, only the server 0 has a 'state' = 2, and it seems that this is the ID of the leader.
    In my scenario, where there is a fully functioning distributed system, and using the Raft consensus algorithm (primary function is to prevent splitting scenarios), there can only be one leader at a time.



2. Perform a Put request for the key 'a' on the leader. What is the new status? What changes occurred and why (if any)?

    Ans: 
    Server 0: 
    {'version': '0.3.12', 'revision': 'deprecated', 'self': TCPNode('127.0.0.1:6000'), 'state': 2, 'leader': TCPNode('127.0.0.1:6000'), 'has_quorum': True, 'partner_nodes_count': 2, 'partner_node_status_server_127.0.0.1:6001': 2, 'partner_node_status_server_127.0.0.1:6002': 2, 'readonly_nodes_count': 0, 'log_len': 3, 'last_applied': 3, 'commit_idx': 3, 'raft_term': 1, 'next_node_idx_count': 2, 'next_node_idx_server_127.0.0.1:6001': 4, 'next_node_idx_server_127.0.0.1:6002': 4, 'match_idx_count': 2, 'match_idx_server_127.0.0.1:6001': 3, 'match_idx_server_127.0.0.1:6002': 3, 'leader_commit_idx': 3, 'uptime': 893, 'self_code_version': 0, 'enabled_code_version': 0}
    
    After sending the Put request, I received a '204 No Response' code, meaning that my request was successfully processed.
    
    The following states have been increased by 1: 'log_len': 3, 'last_applied': 3, 'commit_idx': 3, 'next_node_idx_server_127.0.0.1:6001': 4, 'next_node_idx_server_127.0.0.1:6002': 4, 'match_idx_server_127.0.0.1:6001': 3, 'match_idx_server_127.0.0.1:6002': 3, 'leader_commit_idx': 3.
    Additionally, the uptime has increased, but this just depends on how long I am running the server.
    
    Changes and their meaning:
    Increment in log_len: Indicates a new log entry was added, reflecting the processing of the Put request.
    Increment in last_applied: Shows the latest log entry has been applied to the server's state.
    Increment in commit_idx: Represents the highest log entry known to be committed, confirming the successful commitment of the new entry.
    Increase in next_node_idx_server_127.0.0.1:6001 and next_node_idx_server_127.0.0.1:6002: Implies the leader server is prepared to send subsequent log entries to these follower servers.
    Increase in match_idx_server_127.0.0.1:6001 and match_idx_server_127.0.0.1:6002: Indicates that the new log entry has been successfully replicated on the follower servers.
    Increment in leader_commit_idx: Aligns with the commit_idx, showing the highest log entry committed by the leader.
    
    The reason why the changes occurred is primarily due to how the Raft consensus algorithm functions. The algorithm ensures that all changes are replicated across the cluster's nodes, maintaining consistency and durability of the data. The leader node plays a central role in this process by receiving write requests, updating its log, and then replicating these changes to the follower nodes.
    

3. Perform an Append request for the key 'a' on the leader. What is the new status? What changes occurred and why (if any)?

    Ans: 
    Server 0: {'version': '0.3.12', 'revision': 'deprecated', 'self': TCPNode('127.0.0.1:6000'), 'state': 2, 'leader': TCPNode('127.0.0.1:6000'), 'has_quorum': True, 'partner_nodes_count': 2, 'partner_node_status_server_127.0.0.1:6001': 2, 'partner_node_status_server_127.0.0.1:6002': 2, 'readonly_nodes_count': 0, 'log_len': 4, 'last_applied': 4, 'commit_idx': 4, 'raft_term': 1, 'next_node_idx_count': 2, 'next_node_idx_server_127.0.0.1:6001': 5, 'next_node_idx_server_127.0.0.1:6002': 5, 'match_idx_count': 2, 'match_idx_server_127.0.0.1:6001': 4, 'match_idx_server_127.0.0.1:6002': 4, 'leader_commit_idx': 4, 'uptime': 1746, 'self_code_version': 0, 'enabled_code_version': 0}
        
    After sending the Append request, I received a 204 No Response code, meaning that my request was successfully processed.
        
    The following states have been increased by 1: 'log_len': 4, 'last_applied': 4, 'commit_idx': 4, 'next_node_idx_server_127.0.0.1:6001': 5, 'next_node_idx_server_127.0.0.1:6002': 5, 'match_idx_server_127.0.0.1:6001': 4, 'match_idx_server_127.0.0.1:6002': 4, 'leader_commit_idx': 4.
    Additionally, the uptime has increased, but this just depends on how long I am running the server.
    As the same entries were changed as in the put request, I will not rewrite what each change means.


4. Perform a Get request for the key 'a' on the leader. What is the new status? What change (if any) happened and why?

    Ans: 
    The response code changed from a 204 to a 200 code (which then includes a message) and then I also see the list corresponding to the key 'a'. 
    Besides this, nothing really changes except for the uptime.


# Task 3

1. Shut down the server that acts as a leader. Report the status that you get from the servers that remain active after shutting down the leader.

    Ans:
    After shutting down the leader, here are the states of the two servers (followers):
    server 1: {'version': '0.3.12', 'revision': 'deprecated', 'self': TCPNode('127.0.0.1:6001'), 'state': 0, 'leader': TCPNode('127.0.0.1:6002'), 'has_quorum': True, 'partner_nodes_count': 2, 'partner_node_status_server_127.0.0.1:6002': 2, 'partner_node_status_server_127.0.0.1:6000': 0, 'readonly_nodes_count': 0, 'log_len': 3, 'last_applied': 3, 'commit_idx': 3, 'raft_term': 3, 'next_node_idx_count': 0, 'match_idx_count': 0, 'leader_commit_idx': 3, 'uptime': 46, 'self_code_version': 0, 'enabled_code_version': 0}
    server 2: {'version': '0.3.12', 'revision': 'deprecated', 'self': TCPNode('127.0.0.1:6002'), 'state': 2, 'leader': TCPNode('127.0.0.1:6002'), 'has_quorum': True, 'partner_nodes_count': 2, 'partner_node_status_server_127.0.0.1:6001': 2, 'partner_node_status_server_127.0.0.1:6000': 0, 'readonly_nodes_count': 0, 'log_len': 3, 'last_applied': 3, 'commit_idx': 3, 'raft_term': 3, 'next_node_idx_count': 2, 'next_node_idx_server_127.0.0.1:6001': 4, 'next_node_idx_server_127.0.0.1:6000': 3, 'match_idx_count': 2, 'match_idx_server_127.0.0.1:6001': 3, 'match_idx_server_127.0.0.1:6000': 0, 'leader_commit_idx': 3, 'uptime': 64, 'self_code_version': 0, 'enabled_code_version': 0}
    The 'server 2' has taken over the role of being a leader. This can be seen in the state and in the 'leader' keyword, where the IP address of the 'server 2' is indicated on both nodes. The 'server 1' remains a follower and its state has not changes.


2. Perform a Put request for the key "a". Then, restart the server from the previous point, and indicate the new status for the three servers. Indicate the result of a Get request for the key ``a" to the previous leader.

    Ans: 
    New statuses:
    server 0: {'version': '0.3.12', 'revision': 'deprecated', 'self': TCPNode('127.0.0.1:6000'), 'state': 0, 'leader': TCPNode('127.0.0.1:6002'), 'has_quorum': True, 'partner_nodes_count': 2, 'partner_node_status_server_127.0.0.1:6001': 2, 'partner_node_status_server_127.0.0.1:6002': 2, 'readonly_nodes_count': 0, 'log_len': 3, 'last_applied': 4, 'commit_idx': 4, 'raft_term': 3, 'next_node_idx_count': 0, 'match_idx_count': 0, 'leader_commit_idx': 4, 'uptime': 40, 'self_code_version': 0, 'enabled_code_version': 0} --> state: follower
    
    server 1: {'version': '0.3.12', 'revision': 'deprecated', 'self': TCPNode('127.0.0.1:6001'), 'state': 0, 'leader': TCPNode('127.0.0.1:6002'), 'has_quorum': True, 'partner_nodes_count': 2, 'partner_node_status_server_127.0.0.1:6002': 2, 'partner_node_status_server_127.0.0.1:6000': 2, 'readonly_nodes_count': 0, 'log_len': 3, 'last_applied': 4, 'commit_idx': 4, 'raft_term': 3, 'next_node_idx_count': 0, 'match_idx_count': 0, 'leader_commit_idx': 4, 'uptime': 704, 'self_code_version': 0, 'enabled_code_version': 0} --> state: follower
    
    server 2: {'version': '0.3.12', 'revision': 'deprecated', 'self': TCPNode('127.0.0.1:6002'), 'state': 2, 'leader': TCPNode('127.0.0.1:6002'), 'has_quorum': True, 'partner_nodes_count': 2, 'partner_node_status_server_127.0.0.1:6001': 2, 'partner_node_status_server_127.0.0.1:6000': 2, 'readonly_nodes_count': 0, 'log_len': 3, 'last_applied': 4, 'commit_idx': 4, 'raft_term': 3, 'next_node_idx_count': 2, 'next_node_idx_server_127.0.0.1:6001': 5, 'next_node_idx_server_127.0.0.1:6000': 5, 'match_idx_count': 2, 'match_idx_server_127.0.0.1:6001': 4, 'match_idx_server_127.0.0.1:6000': 4, 'leader_commit_idx': 4, 'uptime': 725, 'self_code_version': 0, 'enabled_code_version': 0} --> state: leader
    
    Result of Get request to previous leader (server 0): ["cat","dog"]


3. Has the Put request been replicated? Indicate which steps lead to a new election and which ones do not. Justify your answer using the statuses returned by the servers.

    Ans: 
    Yes, the put request has been replicated. This can be seen in the new status of the previous leader (server 1). To be precise, the leader 
    has been updates as well as the raft term. Additionally, we can observe that the put request has been replicated when executing the get request 
    on the server 0 and still getting the up-to-date values in the list corresponding to that key. When sending a request to the leader, the 
    request will automatically be forwarded into the Server cluster, where all the servers execute the request. Now we can see that all servers 
    have the same values for the "commit_idx" and "last_applied" fields. This means that the latest log entry has been applied to state machine of 
    each server.
    
    In our example presumably Server 2 timed out and started an election. After receiving a majority of the votes it takes over the
    leader role. The newly elected leader now listens to incoming requests and replicates, then forwards them into the server cluster.
    
    There are different possible scenarios in which a new election can happen. First when there is no heartbeat from the leader
    (either due to a timeout or him stepping down as a leader), when there is no transition into a leader state during a re-election 
    due to a timeout, when a split vote between two candidates happens and another reason would also be the network partitioning.
    
    Getting a normal heartbeat from the leader, replicating the leaders log and the replication of the PUT-request onto the followers
    do not lead to a new election.


4. Shut down two servers, including the leader --- starting with the server that is not the leader. Report the status of the remaining servers and explain what happened.

    Ans:
    status of remaining server (server 1): {'version': '0.3.12', 'revision': 'deprecated', 'self': TCPNode('127.0.0.1:6001'), 'state': 0, 'leader': TCPNode('127.0.0.1:6002'), 'has_quorum': False, 'partner_nodes_count': 2, 'partner_node_status_server_127.0.0.1:6002': 0, 'partner_node_status_server_127.0.0.1:6000': 0, 'readonly_nodes_count': 0, 'log_len': 2, 'last_applied': 4, 'commit_idx': 4, 'raft_term': 3, 'next_node_idx_count': 0, 'match_idx_count': 0, 'leader_commit_idx': 4, 'uptime': 1097, 'self_code_version': 0, 'enabled_code_version': 0}
    
    changes: status for partner_node_status_server_127.0.0.1:6000 and partner_node_status_server_127.0.0.1:6002 change from 2 to 0 and the 'has_quorum' changes from True to False.

    What happened is that 2 out of 3 servers left the cluster, which means that only server one is left. The remaining server is in a 
    state where it cannot become a leader. In other words, it is not able to make any decisions nor is it able to process new log entries. 
    To do so it requires other servers to communicate with. Now our system is not operational anymore. To restore its functionality we do
    need to restart the servers, which we turned off before. 


5. Can you perform Get, Put, or Append requests in this system state? Justify your answer.

    Ans:
    Executing a GET-request on our remaining server is possible since it doesn't require any changes to the state machine.
    On the other hand the PUT- and APPEND- requests won't work, since they require a consensus across the majority of the servers
    in the cluster. Since we are not able to fulfill this requirement, the write-operations cannot be executed. 


6. Restart the servers and note down the new status. Describe what happened.

    Ans:
    new status of server 1: {'version': '0.3.12', 'revision': 'deprecated', 'self': TCPNode('127.0.0.1:6001'), 'state': 2, 'leader': TCPNode('127.0.0.1:6001'), 'has_quorum': True, 'partner_nodes_count': 2, 'partner_node_status_server_127.0.0.1:6002': 2, 'partner_node_status_server_127.0.0.1:6000': 2, 'readonly_nodes_count': 0, 'log_len': 3, 'last_applied': 5, 'commit_idx': 5, 'raft_term': 4, 'next_node_idx_count': 2, 'next_node_idx_server_127.0.0.1:6002': 6, 'next_node_idx_server_127.0.0.1:6000': 6, 'match_idx_count': 2, 'match_idx_server_127.0.0.1:6002': 5, 'match_idx_server_127.0.0.1:6000': 5, 'leader_commit_idx': 5, 'uptime': 1620, 'self_code_version': 0, 'enabled_code_version': 0}

    After restarting the servers, a new election took place in which server one became the new leader. Now that the cluster regained
    its quorum, we can process all operations again. Server 1 is now responsible for maintaining the state of the cluster and for
    managing the log replication. Our log indices went up which means that our cluster has processed new operations. In this case
    the leader election could be one of them.


# Task 4

1. What is a consensus algorithm? What are they used for in the context of replicated state machines? 

    Ans: 
    A consensus algorithm is a set of rules and procedures that allows a group nodes/servers to reach an agreement on 
    a single data value or a sequence of values. The primary goal of a consensus algorithm is to ensure that all participating 
    nodes agree on the same value, even in the presence of faults or failures.
        
    In the context of replicated state machines, consensus algorithms helps achieve fault tolerance and ensures consistency 
    across the distributed system. Replicated state machines are a general approach to building fault-tolerant systems, where 
    each server in a cluster maintains a copy of a state machine and a log of commands. The state machine is the component that 
    needs to be made fault-tolerant, and the log contains a series of commands that are applied to the state machine.


2. What are the main features of the Raft algorithm? How does Raft enable fault tolerance?

    Ans: 
    Raft organizes nodes into leaders, followers, and candidates, with a leader responsible for log replication and decision-making. 
    Operating in terms, Raft ensures a consistent leader through periodic heartbeats and leader elections. The leader's append-only 
    property prevents conflicts, as only the leader can add entries to its log. Log replication, coupled with a majority-based approach, 
    ensures that the system can continue functioning even when some nodes fail. The leader completeness property guarantees safety 
    by ensuring that committed log entries persist across leader changes.


3. What are Byzantine failures? Can Raft handle them?

    Ans: 
    Byzantine failures are a type of failure in a distributed system where a component behaves maliciously. In other words, 
    nodes affected by Byzantine failures may have unpredictable behavior, including sending incorrect or conflicting information, 
    providing inconsistent responses, or even intentionally attempting to disrupt the operation of the system. The Raft consensus 
    algorithm assumes a non-malicious environment where nodes may fail but are assumed to behave correctly when they are operational. 
    Therefore, Raft cannot handle Byzantine failures.

# Support 
https://raft.github.io
http://thesecretlivesofdata.com/raft/
https://www.geeksforgeeks.org/raft-consensus-algorithm/
https://medium.com/coinmonks/the-raft-algorithm-achieving-distributed-systems-consensus-e8c17542699b
https://chat.openai.com