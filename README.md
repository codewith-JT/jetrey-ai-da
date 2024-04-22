














import java.util.Random;

public class QLearningExample {
    // Define the grid world environment
    private static final int NUM_STATES = 6;
    private static final int NUM_ACTIONS = 2;
    private static final double GAMMA = 0.8; // Discount factor
    private static final double LEARNING_RATE = 0.1;
    private static final int MAX_EPISODES = 1000;

    private static final int[][] REWARDS = {
            {-1, -1}, // State 0: Actions (0: Left, 1: Right)
            {-1, -1}, // State 1
            {-1, -1}, // State 2
            {-1, -1}, // State 3
            {-1, -1}, // State 4
            {-1, 10}  // State 5: Goal state
    };

    private static final Random random = new Random();

    public static void main(String[] args) {
        double[][] QValues = new double[NUM_STATES][NUM_ACTIONS];

        // Q-learning algorithm
        for (int episode = 0; episode < MAX_EPISODES; episode++) {
            int currentState = random.nextInt(NUM_STATES); // Start from a random state

            while (currentState != NUM_STATES - 1) { // Continue until goal state is reached
                int action = chooseAction(currentState, QValues);
                int nextState = getNextState(currentState, action);
                double reward = getReward(currentState, action);

                // Update Q-value using the Q-learning formula
                QValues[currentState][action] = QValues[currentState][action] +
                        LEARNING_RATE * (reward + GAMMA * maxQValue(nextState, QValues) - QValues[currentState][action]);

                currentState = nextState; // Move to the next state
            }
        }

        // Print the learned Q-values
        System.out.println("Learned Q-values:");
        for (int i = 0; i < NUM_STATES; i++) {
            for (int j = 0; j < NUM_ACTIONS; j++) {
                System.out.printf("Q(%d, %d) = %.2f\n", i, j, QValues[i][j]);
            }
        }
    }

    private static int chooseAction(int state, double[][] QValues) {
        if (random.nextDouble() < 0.5) {
            return 0; // Choose left with 50% probability
        } else {
            return 1; // Choose right with 50% probability
        }
    }

    private static int getNextState(int currentState, int action) {
        if (action == 0) {
            return Math.max(currentState - 1, 0); // Move left
        } else {
            return Math.min(currentState + 1, NUM_STATES - 1); // Move right
        }
    }

    private static double getReward(int state, int action) {
        return REWARDS[state][action];
    }

    private static double maxQValue(int state, double[][] QValues) {
        return Math.max(QValues[state][0], QValues[state][1]);
    }
}
