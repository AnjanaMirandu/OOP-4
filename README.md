
<?php

class CollatzCalculator {
    public function calculate($x) {
        $count = 0;
        while ($x != 1) {
            if ($x % 2 == 0) {
                $x /= 2;
            } else {
                $x = 3 * $x + 1;
            }
            $count++;
        }
        return $count;
    }
}

class RangeAnalyzer {
    private $calculator;

    public function __construct() {
        $this->calculator = new CollatzCalculator();
    }

    public function analyzeRange($start, $end) {
        $maxValue = 0;
        $minValue = PHP_INT_MAX;
        $maxNumbers = [];
        $minNumbers = [];
        $totalIterations = 0;

        for ($i = $start; $i <= $end; $i++) {
            $currentIterations = $this->calculator->calculate($i);
            $totalIterations += $currentIterations;

            if ($currentIterations > $maxValue) {
                $maxValue = $currentIterations;
                $maxNumbers = [$i];
            } elseif ($currentIterations == $maxValue) {
                $maxNumbers[] = $i;
            }

            if ($currentIterations < $minValue) {
                $minValue = $currentIterations;
                $minNumbers = [$i];
            } elseif ($currentIterations == $minValue) {
                $minNumbers[] = $i;
            }
        }

        return [
            'max_value' => $maxValue,
            'max_numbers' => $maxNumbers,
            'min_value' => $minValue,
            'min_numbers' => $minNumbers,
            'total_iterations' => $totalIterations
        ];
    }
}

// Prompt user for range values
$startRange = (int)readline("Enter the start of the range: ");
$endRange = (int)readline("Enter the end of the range: ");

// Find max and min values and numbers with max and min iterations for range
echo "\nFinding max and min values and numbers with max and min iterations for range $startRange to $endRange\n";
$analyzer = new RangeAnalyzer();
$result = $analyzer->analyzeRange($startRange, $endRange);
echo "Max value: " . $result['max_value'] . "\n";
echo "Numbers achieving max value: " . implode(", ", $result['max_numbers']) . "\n";
echo "Min value: " . $result['min_value'] . "\n";
echo "Numbers achieving min value: " . implode(", ", $result['min_numbers']) . "\n";
echo "Total iterations: " . $result['total_iterations'] . "\n";
?>

