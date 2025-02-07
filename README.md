import java.util.Arrays;
import java.util.Random;
import java.util.concurrent.ForkJoinPool;
import java.util.concurrent.RecursiveAction;

public class Main {
    public static void main(String[] args) {
        int[] array1 = generateRandomArray(4000000);
        int[] array2 = Arrays.copyOf(array1, array1.length);

        // Ordenamiento Secuencial
        long startTime = System.nanoTime();
        Arrays.sort(array1);
        long endTime = System.nanoTime();
        System.out.println("Sequential sorting time: " + (endTime - startTime) / 1_000_000 + " ms");

        // Ordenamiento Paralelo
        ForkJoinPool pool = new ForkJoinPool();
        startTime = System.nanoTime();
        pool.invoke(new SortTask(array2));
        endTime = System.nanoTime();
        System.out.println("Parallel sorting time: " + (endTime - startTime) / 1_000_000 + " ms");
    }

    private static int[] generateRandomArray(int size) {
        Random random = new Random();
        int[] array = new int[size];
        for (int i = 0; i < size; i++) {import java.util.Arrays;
import java.util.Random;
import java.util.concurrent.ForkJoinPool;
import java.util.concurrent.RecursiveAction;

public class Main {
    private static final String[] SUITS = {"Hearts", "Diamonds", "Clubs", "Spades"};
    private static final String[] RANKS = {"1", "2", "3", "4", "5", "6", "7", "8", "9", "10", "11", "12", "13", "Joker"};

    public static void main(String[] args) {
        Card[] deck1 = generateDeck(4000000);
        Card[] deck2 = Arrays.copyOf(deck1, deck1.length);

        // Ordenamiento Secuencial
        long startTime = System.nanoTime();
        Arrays.sort(deck1);
        long endTime = System.nanoTime();
        System.out.println("Sequential sorting time: " + (endTime - startTime) / 1_000_000 + " ms");

        // Ordenamiento Paralelo
        ForkJoinPool pool = new ForkJoinPool();
        startTime = System.nanoTime();
        pool.invoke(new SortTask(deck2));
        endTime = System.nanoTime();
        System.out.println("Parallel sorting time: " + (endTime - startTime) / 1_000_000 + " ms");
    }

    private static Card[] generateDeck(int size) {
        Random random = new Random();
        Card[] deck = new Card[size];
        for (int i = 0; i < size; i++) {
            String suit = SUITS[random.nextInt(SUITS.length)];
            String rank = RANKS[random.nextInt(RANKS.length)];
            deck[i] = new Card(suit, rank);
        }
        return deck;
    }

    static class Card implements Comparable<Card> {
        String suit;
        String rank;

        Card(String suit, String rank) {
            this.suit = suit;
            this.rank = rank;
        }

        @Override
        public int compareTo(Card other) {
            int rankComparison = Integer.compare(getRankValue(this.rank), getRankValue(other.rank));
            if (rankComparison != 0) {
                return rankComparison;
            } else {
                return this.suit.compareTo(other.suit);
            }
        }

        private int getRankValue(String rank) {
            if (rank.equals("Joker")) {
                return 14;
            }
            return Integer.parseInt(rank);
        }
    }

    static class SortTask extends RecursiveAction {
        private static final int THRESHOLD = 1000;
        private final Card[] array;
        private final int start, end;

        SortTask(Card[] array) {
            this(array, 0, array.length);
        }

        SortTask(Card[] array, int start, int end) {
            this.array = array;
            this.start = start;
            this.end = end;
        }

        @Override
        protected void compute() {
            if (end - start <= THRESHOLD) {
                Arrays.sort(array, start, end);
            } else {
                int mid = start + (end - start) / 2;
                invokeAll(new SortTask(array, start, mid), new SortTask(array, mid, end));
            }
        }
    }
}

            array[i] = random.nextInt();
        }
        return array;
    }

    static class SortTask extends RecursiveAction {
        private static final int THRESHOLD = 1000;
        private final int[] array;
        private final int start, end;

        SortTask(int[] array) {
            this(array, 0, array.length);
        }

        SortTask(int[] array, int start, int end) {
            this.array = array;
            this.start = start;
            this.end = end;
        }

        @Override
        protected void compute() {
            if (end - start <= THRESHOLD) {
                Arrays.sort(array, start, end);
            } else {
                int mid = start + (end - start) / 2;
                invokeAll(new SortTask(array, start, mid), new SortTask(array, mid, end));
            }
        }
    }
}
