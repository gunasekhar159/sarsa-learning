# SARSA Learning Algorithm


## AIM
To develop a Python program to find the optimal policy for the given RL environment using SARSA-Learning and compare the state values with the Monte Carlo method.

## PROBLEM STATEMENT
Design and implement a Python program to explore and exploit the optimal policy in a given reinforcement learning (RL) environment using SARSA-Learning algorithm. The program should iteratively update state-action values based on experiences gained from interactions with the environment, aiming to converge towards the optimal policy. Furthermore, the implementation should include mechanisms for policy evaluation and improvement using SARSA. Additionally, conduct a comparative analysis by evaluating state values obtained through SARSA-Learning against those obtained through the Monte Carlo method, highlighting similarities, differences, and performance metrics to assess the effectiveness of both approaches in solving the RL problem.s.

## SARSA LEARNING ALGORITHM
Step 1:
Initialize the Q-table with random values for all state-action pairs.

Step 2:
Initialize the current state S and choose the initial action A using an epsilon-greedy policy based on the Q-values in the Q-table.

Step 3:
Repeat until the episode ends and then take action A and observe the next state S' and the reward R.

Step 4:
Update the Q-value for the current state-action pair (S, A) using the SARSA update rule.

Step 5:
Update State and Action and repeat the step 3 untill the episodes ends.
## SARSA LEARNING FUNCTION
```
Developed by : M.Gunasekhar
Ref No :21222124014

def sarsa(env,
          gamma=1.0,
          init_alpha=0.5,
          min_alpha=0.01,
          alpha_decay_ratio=0.5,
          init_epsilon=1.0,
          min_epsilon=0.1,
          epsilon_decay_ratio=0.9,
          n_episodes=3000):
    nS, nA = env.observation_space.n, env.action_space.n
    pi_track = []
    Q = np.zeros((nS, nA), dtype=np.float64)
    Q_track = np.zeros((n_episodes, nS, nA), dtype=np.float64)
    select_action = lambda state,Q,epsilon: np.argmax(Q[state]) if np.random.random()>epsilon else np.random.randint(len(Q[state]))
    alphas=decay_schedule(init_alpha,min_alpha,alpha_decay_ratio,n_episodes)

    epsilons=decay_schedule(init_epsilon,min_epsilon,epsilon_decay_ratio,n_episodes)
    for e in tqdm(range(n_episodes),leave=False):
      state,done=env.reset(),False
      action=select_action(state,Q,epsilons[e])
      while not done:
        next_state,reward,done,_=env.step(action)
        next_action=select_action(next_state,Q,epsilons[e])
        td_target=reward+gamma*Q[next_state][next_action]*(not done)
        td_error=td_target-Q[state][action]
        Q[state][action]=Q[state][action]+alphas[e]*td_error
        state,action=next_state,next_action
      Q_track[e]=Q
      pi_track.append(np.argmax(Q,axis=1))
    V=np.max(Q,axis=1)
    pi=lambda s:{s:a for s,a in enumerate(np.argmax(Q,axis=1))}[s]
    return Q, V, pi, Q_track, pi_track
```
## OUTPUT:
Mention the optimal policy, optimal value function , success rate for the optimal policy.
<img width="590" alt="image" src="https://github.com/gunasekhar159/sarsa-learning/assets/95043391/e418868c-3f2d-4c81-9bfc-c85e26a89236">
<img width="740" alt="image" src="https://github.com/gunasekhar159/sarsa-learning/assets/95043391/87f0039f-bda4-445b-b1ff-8c048aea037e">
<img width="691" alt="image" src="https://github.com/gunasekhar159/sarsa-learning/assets/95043391/0496b70c-9668-4776-be6d-fe31001a8d9b">


## RESULT:

Therefore a python has been successfully developed to find the optimal policy for the given rl environment using SARSA-Learning and state values are compared with Monte Carlo method.

