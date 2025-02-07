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
        for (int i = 0; i < size; i++) {
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
