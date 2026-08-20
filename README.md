EX:NO:05: Develop a program to create a simple calculator using android studio.
AIM:
To create and design an android application for a simple calculator using android studio.

EQUIPMENTS REQUIRED:
Android Studio(Latest Version)

ALGORITHM:


Step 1: Open Android Studio and click on File → New → New Project.

Step 2: Enter the application name as Calculator, select the required Minimum SDK, and click Finish.

Step 3: Select Empty Activity and create the Android project.

Step 4: Design the calculator interface with number and operator buttons in activity_main.xml.

Step 5: Implement addition, subtraction, multiplication, division, clear, delete, and equals operations in MainActivity.java.

Step 6: Use Implicit Intent with ACTION_SEND to share the calculated result with other applications.

Step 7: Save and run the application, perform calculations, and verify the result and sharing functionality.

PROGRAM:
```

Program to create and design an android application simple calculator using Intent.
Developed by: NITHISH S
Registeration Number : 212223220070
```
## Main Activity.java
```
package com.example.calculator;

import android.os.Bundle;
import android.view.View;
import android.widget.Button;

import androidx.activity.EdgeToEdge;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.graphics.Insets;
import androidx.core.view.ViewCompat;
import androidx.core.view.WindowInsetsCompat;

import com.example.calculator.R;
import com.example.calculator.databinding.ActivityMainBinding;

import java.util.Locale;
import java.util.Objects;

public class MainActivity extends AppCompatActivity {

    private ActivityMainBinding binding;
    private double firstValue = Double.NaN;
    private String currentOperator = null;
    private boolean isNewOp = true;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        EdgeToEdge.enable(this);
        binding = ActivityMainBinding.inflate(getLayoutInflater());
        setContentView(binding.getRoot());

        ViewCompat.setOnApplyWindowInsetsListener(binding.main, (v, insets) -> {
            Insets systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars());
            v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom);
            return insets;
        });

        setNumberListeners();
        setOperatorListeners();

        binding.btnC.setOnClickListener(v -> {
            firstValue = Double.NaN;
            currentOperator = null;
            isNewOp = true;
            binding.tvDisplay.setText(R.string.digit_0);
        });

        binding.btnEqual.setOnClickListener(v -> calculate());

        binding.btnDot.setOnClickListener(v -> {
            String current = binding.tvDisplay.getText().toString();
            if (Objects.equals(current, getString(R.string.error))) {
                binding.tvDisplay.setText("0.");
                isNewOp = false;
            } else if (!current.contains(".")) {
                binding.tvDisplay.append(".");
                isNewOp = false;
            }
        });
    }

    private void setNumberListeners() {
        View.OnClickListener listener = v -> {
            Button b = (Button) v;
            String text = b.getText().toString();
            String current = binding.tvDisplay.getText().toString();

            if (isNewOp || Objects.equals(current, getString(R.string.error))) {
                binding.tvDisplay.setText(text);
                isNewOp = false;
            } else {
                if (Objects.equals(current, getString(R.string.digit_0))) {
                    binding.tvDisplay.setText(text);
                } else {
                    binding.tvDisplay.append(text);
                }
            }
        };

        binding.btn0.setOnClickListener(listener);
        binding.btn1.setOnClickListener(listener);
        binding.btn2.setOnClickListener(listener);
        binding.btn3.setOnClickListener(listener);
        binding.btn4.setOnClickListener(listener);
        binding.btn5.setOnClickListener(listener);
        binding.btn6.setOnClickListener(listener);
        binding.btn7.setOnClickListener(listener);
        binding.btn8.setOnClickListener(listener);
        binding.btn9.setOnClickListener(listener);
    }

    private void setOperatorListeners() {
        View.OnClickListener listener = v -> {
            Button b = (Button) v;
            String current = binding.tvDisplay.getText().toString();
            
            if (Objects.equals(current, getString(R.string.error))) return;

            if (!Double.isNaN(firstValue) && currentOperator != null && !isNewOp) {
                calculate();
            }
            
            try {
                firstValue = Double.parseDouble(binding.tvDisplay.getText().toString());
                currentOperator = b.getText().toString();
                isNewOp = true;
            } catch (NumberFormatException e) {
                binding.tvDisplay.setText(R.string.error);
            }
        };

        binding.btnAdd.setOnClickListener(listener);
        binding.btnMinus.setOnClickListener(listener);
        binding.btnMultiply.setOnClickListener(listener);
        binding.btnDivide.setOnClickListener(listener);
    }

    private void calculate() {
        if (Double.isNaN(firstValue) || currentOperator == null) return;

        String current = binding.tvDisplay.getText().toString();
        double secondValue;
        try {
            secondValue = Double.parseDouble(current);
        } catch (NumberFormatException e) {
            return;
        }

        double result;
        switch (currentOperator) {
            case "+":
                result = firstValue + secondValue;
                break;
            case "-":
                result = firstValue - secondValue;
                break;
            case "*":
                result = firstValue * secondValue;
                break;
            case "/":
                if (secondValue != 0) {
                    result = firstValue / secondValue;
                } else {
                    binding.tvDisplay.setText(R.string.error);
                    firstValue = Double.NaN;
                    currentOperator = null;
                    isNewOp = true;
                    return;
                }
                break;
            default:
                return;
        }

        String resultStr;
        if (result == (long) result) {
            resultStr = String.format(Locale.getDefault(), "%d", (long) result);
        } else {
            resultStr = String.format(Locale.getDefault(), "%s", result);
        }

        binding.tvDisplay.setText(resultStr);
        firstValue = result;
        currentOperator = null;
        isNewOp = true;
    }
}
```
## Activity_Main.xml
```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="@color/black"
    android:orientation="vertical"
    android:padding="16dp"
    tools:context="com.example.calculator.MainActivity">

    <TextView
        android:id="@+id/tvDisplay"
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_weight="1"
        android:gravity="bottom|end"
        android:padding="16dp"
        android:text="@string/digit_0"
        android:textColor="@color/white"
        android:textSize="64sp" />

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal">

        <Button
            android:id="@+id/btnC"
            style="@style/Widget.Material3.Button.TonalButton"
            android:layout_width="0dp"
            android:layout_height="80dp"
            android:layout_margin="4dp"
            android:layout_weight="1"
            android:text="@string/op_clear"
            android:textSize="24sp" />

        <Button
            android:id="@+id/btnDivide"
            style="@style/Widget.Material3.Button.TonalButton"
            android:layout_width="0dp"
            android:layout_height="80dp"
            android:layout_margin="4dp"
            android:layout_weight="1"
            android:text="@string/op_divide"
            android:textColor="@color/orange"
            android:textSize="24sp" />
    </LinearLayout>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal">

        <Button
            android:id="@+id/btn7"
            style="@style/Widget.Material3.Button.TonalButton"
            android:layout_width="0dp"
            android:layout_height="80dp"
            android:layout_margin="4dp"
            android:layout_weight="1"
            android:text="@string/digit_7"
            android:textSize="24sp" />

        <Button
            android:id="@+id/btn8"
            style="@style/Widget.Material3.Button.TonalButton"
            android:layout_width="0dp"
            android:layout_height="80dp"
            android:layout_margin="4dp"
            android:layout_weight="1"
            android:text="@string/digit_8"
            android:textSize="24sp" />

        <Button
            android:id="@+id/btn9"
            style="@style/Widget.Material3.Button.TonalButton"
            android:layout_width="0dp"
            android:layout_height="80dp"
            android:layout_margin="4dp"
            android:layout_weight="1"
            android:text="@string/digit_9"
            android:textSize="24sp" />

        <Button
            android:id="@+id/btnMultiply"
            style="@style/Widget.Material3.Button.TonalButton"
            android:layout_width="0dp"
            android:layout_height="80dp"
            android:layout_margin="4dp"
            android:layout_weight="1"
            android:text="@string/op_multiply"
            android:textColor="@color/orange"
            android:textSize="24sp" />
    </LinearLayout>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal">

        <Button
            android:id="@+id/btn4"
            style="@style/Widget.Material3.Button.TonalButton"
            android:layout_width="0dp"
            android:layout_height="80dp"
            android:layout_margin="4dp"
            android:layout_weight="1"
            android:text="@string/digit_4"
            android:textSize="24sp" />

        <Button
            android:id="@+id/btn5"
            style="@style/Widget.Material3.Button.TonalButton"
            android:layout_width="0dp"
            android:layout_height="80dp"
            android:layout_margin="4dp"
            android:layout_weight="1"
            android:text="@string/digit_5"
            android:textSize="24sp" />

        <Button
            android:id="@+id/btn6"
            style="@style/Widget.Material3.Button.TonalButton"
            android:layout_width="0dp"
            android:layout_height="80dp"
            android:layout_margin="4dp"
            android:layout_weight="1"
            android:text="@string/digit_6"
            android:textSize="24sp" />

        <Button
            android:id="@+id/btnMinus"
            style="@style/Widget.Material3.Button.TonalButton"
            android:layout_width="0dp"
            android:layout_height="80dp"
            android:layout_margin="4dp"
            android:layout_weight="1"
            android:text="@string/op_subtract"
            android:textColor="@color/orange"
            android:textSize="24sp" />
    </LinearLayout>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal">

        <Button
            android:id="@+id/btn1"
            style="@style/Widget.Material3.Button.TonalButton"
            android:layout_width="0dp"
            android:layout_height="80dp"
            android:layout_margin="4dp"
            android:layout_weight="1"
            android:text="@string/digit_1"
            android:textSize="24sp" />

        <Button
            android:id="@+id/btn2"
            style="@style/Widget.Material3.Button.TonalButton"
            android:layout_width="0dp"
            android:layout_height="80dp"
            android:layout_margin="4dp"
            android:layout_weight="1"
            android:text="@string/digit_2"
            android:textSize="24sp" />

        <Button
            android:id="@+id/btn3"
            style="@style/Widget.Material3.Button.TonalButton"
            android:layout_width="0dp"
            android:layout_height="80dp"
            android:layout_margin="4dp"
            android:layout_weight="1"
            android:text="@string/digit_3"
            android:textSize="24sp" />

        <Button
            android:id="@+id/btnAdd"
            style="@style/Widget.Material3.Button.TonalButton"
            android:layout_width="0dp"
            android:layout_height="80dp"
            android:layout_margin="4dp"
            android:layout_weight="1"
            android:text="@string/op_add"
            android:textColor="@color/orange"
            android:textSize="24sp" />
    </LinearLayout>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal">

        <Button
            android:id="@+id/btn0"
            style="@style/Widget.Material3.Button.TonalButton"
            android:layout_width="0dp"
            android:layout_height="80dp"
            android:layout_margin="4dp"
            android:layout_weight="2"
            android:text="@string/digit_0"
            android:textSize="24sp" />

        <Button
            android:id="@+id/btnDot"
            style="@style/Widget.Material3.Button.TonalButton"
            android:layout_width="0dp"
            android:layout_height="80dp"
            android:layout_margin="4dp"
            android:layout_weight="1"
            android:text="@string/dot"
            android:textSize="24sp" />

        <Button
            android:id="@+id/btnEqual"
            style="@style/Widget.Material3.Button.TonalButton"
            android:layout_width="0dp"
            android:layout_height="80dp"
            android:layout_margin="4dp"
            android:layout_weight="1"
            android:text="@string/op_equal"
            android:textColor="@color/orange"
            android:textSize="24sp" />
    </LinearLayout>

</LinearLayout>
```



## Output:
<img width="1919" height="1025" alt="image_5" src="https://github.com/user-attachments/assets/a13adf21-0b53-4185-b2d6-7c8fbf070c9f" />






