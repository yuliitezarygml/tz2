# Android Countdown Timer App

Це мобільний застосунок під Android, який реалізує таймер зворотного відліку від 10 секунд після натиснення кнопки "Start". Застосунок створений на Java з використанням Android SDK.

## Структура проекту

Основні файли:
- `app/src/main/res/layout/activity_countdown.xml` - XML-шаблон інтерфейсу користувача.
- `app/src/main/res/values/strings.xml` - Рядкові ресурси.
- `app/src/main/java/com/example/myapplication/CountdownActivity.java` - Основна логіка активності.
- `app/src/main/AndroidManifest.xml` - Маніфест додатку.

## Построчне пояснення коду

### 1. activity_countdown.xml
```xml
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent" >
```
- `<RelativeLayout>`: Кореневий контейнер, що дозволяє розташовувати елементи відносно один одного.
- `xmlns:android`: Простір імен для атрибутів Android.
- `xmlns:tools`: Простір імен для інструментів розробки (не обов'язковий для роботи).
- `android:layout_width="match_parent"`: Ширина займає всю ширину батьківського елемента.
- `android:layout_height="match_parent"`: Висота займає всю висоту батьківського елемента.

```xml
    <TextView
        android:id="@+id/time_display_box"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentTop="true"
        android:layout_centerHorizontal="true"
        android:layout_marginTop="60dp"
        android:text="@string/_00_10"
        android:textAppearance="?android:attr/textAppearanceLarge"/>
```
- `<TextView>`: Елемент для відображення тексту.
- `android:id="@+id/time_display_box"`: Унікальний ідентифікатор для звернення з коду.
- `android:layout_width="wrap_content"`: Ширина підлаштовується під вміст.
- `android:layout_height="wrap_content"`: Висота підлаштовується під вміст.
- `android:layout_alignParentTop="true"`: Вирівнювання по верхньому краю батьківського елемента.
- `android:layout_centerHorizontal="true"`: Центрування по горизонталі.
- `android:layout_marginTop="60dp"`: Відступ зверху 60 dp (density-independent pixels).
- `android:text="@string/_00_10"`: Текст з ресурсів (рядок "_00_10").
- `android:textAppearance="?android:attr/textAppearanceLarge"`: Використання великого стилю тексту з теми.

```xml
    <Button
        android:id="@+id/startbutton"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_below="@+id/time_display_box"
        android:layout_centerHorizontal="true"
        android:layout_marginTop="41dp"
        android:text="@string/start" />
```
- `<Button>`: Кнопка для взаємодії.
- `android:id="@+id/startbutton"`: Ідентифікатор кнопки.
- `android:layout_width="wrap_content"`: Ширина під вміст.
- `android:layout_height="wrap_content"`: Висота під вміст.
- `android:layout_below="@+id/time_display_box"`: Розташування нижче TextView.
- `android:layout_centerHorizontal="true"`: Центрування по горизонталі.
- `android:layout_marginTop="41dp"`: Відступ зверху 41 dp.
- `android:text="@string/start"`: Текст кнопки з ресурсів.

```xml
</RelativeLayout>
```
- Закриття кореневого елемента.

### 2. strings.xml
```xml
<resources>
    <string name="app_name">My Application</string>
    <string name="start">Start</string>
    <string name="_00_10">00:10</string>
</resources>
```
- `<resources>`: Кореневий елемент для ресурсів.
- `<string name="app_name">My Application</string>`: Назва додатку.
- `<string name="start">Start</string>`: Текст для кнопки.
- `<string name="_00_10">00:10</string>`: Початковий текст для таймера.
- `</resources>`: Закриття.

### 3. CountdownActivity.java
```java
package com.example.myapplication;
```
- Оголошення пакету, до якого належить клас.

```java
import android.app.Activity;
import android.os.Bundle;
import android.os.CountDownTimer;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;
```
- Імпорти необхідних класів Android.

```java
public class CountdownActivity extends Activity {
```
- Оголошення класу, що розширює Activity (базовий клас для екранів).

```java
    private static final int MILLIS_PER_SECOND = 1000;
    private static final int SECONDS_TO_COUNTDOWN = 10;
    private TextView countdownDisplay;
    private CountDownTimer timer;
```
- Константи: мілісекунди в секунді та кількість секунд для відліку.
- Поля: TextView для відображення та CountDownTimer для таймера.

```java
    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_countdown);
```
- Метод onCreate викликається при створенні активності.
- `super.onCreate(savedInstanceState)`: Виклик батьківського методу.
- `setContentView(R.layout.activity_countdown)`: Підключення XML-шаблону.

```java
        countdownDisplay = (TextView) findViewById(R.id.time_display_box);
        Button startButton = (Button) findViewById(R.id.startbutton);
```
- Знаходження елементів інтерфейсу за ідентифікаторами.

```java
        startButton.setOnClickListener(new View.OnClickListener() {
            public void onClick(View view) {
                try {
                    showTimer(SECONDS_TO_COUNTDOWN * MILLIS_PER_SECOND);
                } catch (NumberFormatException e) {
                }
            }
        });
```
- Встановлення обробника натиснення кнопки.
- `new View.OnClickListener()`: Анонімний клас для обробки події.
- `onClick(View view)`: Метод, що викликається при натисненні.
- Виклик `showTimer` з часом у мілісекундах.
- `try-catch`: Обробка винятків (хоча тут не обов'язково).

```java
    }
```
- Закриття методу onCreate.

```java
    private void showTimer(int countdownMillis) {
        if(timer != null) { timer.cancel(); }
```
- Метод для запуску таймера.
- Якщо таймер вже існує, скасувати його.

```java
        timer = new CountDownTimer(countdownMillis, MILLIS_PER_SECOND) {
```
- Створення нового CountDownTimer з часом та інтервалом 1 секунда.

```java
            @Override
            public void onTick(long millisUntilFinished) {
                countdownDisplay.setText("counting down: " +
                    millisUntilFinished / MILLIS_PER_SECOND);
            }
```
- Метод onTick викликається кожну секунду.
- Оновлення тексту з поточними секундами.

```java
            @Override
            public void onFinish() {
                countdownDisplay.setText("PRYVIT!");
            }
```
- Метод onFinish викликається після завершення.
- Встановлення фінального тексту.

```java
        }.start();
```
- Запуск таймера.

```java
    }
}
```
- Закриття методу та класу.

### 4. AndroidManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">
```
- Заголовок XML та оголошення маніфесту.

```xml
    <application
        android:allowBackup="true"
        android:dataExtractionRules="@xml/data_extraction_rules"
        android:fullBackupContent="@xml/backup_rules"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.MyApplication">
```
- `<application>`: Налаштування додатку.
- Атрибути: дозвіл бекапу, іконки, назва, тема тощо.

```xml
        <activity
            android:name=".CountdownActivity"
            android:exported="true"
            android:label="@string/app_name"
            android:theme="@style/Theme.MyApplication">
```
- Оголошення активності.
- `android:name=".CountdownActivity"`: Ім'я класу.
- `android:exported="true"`: Дозвіл експорту (для запуску).

```xml
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
```
- Фільтр інтенту для запуску як головної активності.

```xml
        </activity>
    </application>
</manifest>
```
- Закриття елементів.

## Інструкції по запуску
1. Відкрийте проект в Android Studio.
2. Запустіть емулятор або підключіть пристрій.
3. Натисніть "Run" для збірки та запуску.
4. Натисніть "Start" для початку відліку від 10 секунд.</content>
<parameter name="filePath">c:\Users\yuliitezary\AndroidStudioProjects\MyApplication2\README.md#   t z 2  
 