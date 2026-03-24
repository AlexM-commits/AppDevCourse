Министерство транспорта Российской Федерации
ФЕДЕРАЛЬНОЕ ГОСУДАРСТВЕННОЕ АВТОНОМНОЕ ОБРАЗОВАТЕЛЬНОЕ УЧРЕЖДЕНИЕ ВЫСШЕГО ОБРАЗОВАНИЯ РОССИЙСКИЙ УНИВЕРСИТЕТ ТРАНСПОРТА
РОССИЙСКАЯ открытая академия транспорта


Кафедра «Системы управления транспортной инфраструктурой»





 КУРСОВАЯ РАБОТА
ПО ДИСЦИПЛИНЕ
Разработка мобильных приложений
ВАРИАНТ 2










Выполнил: 
Мелешко А.К.
ЗИИ-532
2110-пПИб-2001











Москва 2025 г.
Цель курсовой работы
Закрепление практических навыков разработки мобильных приложений под платформу Android с использованием современных инструментов и технологий.


Ход выполнения

Данная курсовая работа была выполнена в ПО Android Studio с использованием Github для контроля Git. В ходе работы был создан публичный репозиторий, находящийся по адресу https://github.com/AlexM-commits/AppDevCourse

Исходный код
MainActivity.kt

package com.example.diceroller

import android.os.Bundle
import androidx.activity.enableEdgeToEdge
import androidx.appcompat.app.AppCompatActivity
import androidx.core.view.ViewCompat
import androidx.core.view.WindowInsetsCompat
import android.content.Intent
import android.widget.Button

class MainActivity : AppCompatActivity() {
 override fun onCreate(savedInstanceState: Bundle?) {
 super.onCreate(savedInstanceState)
 enableEdgeToEdge()
 setContentView(R.layout.activity_main)
 ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.main)) { v, insets ->
 val systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars())
 v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom)
 insets
 }
 val rollerButton = findViewById<Button>(R.id.StartButton)
 rollerButton.setOnClickListener {
 startActivity(Intent(this, MainActivityRoller::class.java))
 }
 val exitButton = findViewById<Button>(R.id.buttonExit2)
 exitButton.setOnClickListener {
 finishAffinity()
 }
 }
}

MainActivityRoller.kt

package com.example.diceroller

import android.content.Intent
import android.os.Bundle
import android.widget.Button
import androidx.activity.enableEdgeToEdge
import androidx.appcompat.app.AppCompatActivity
import androidx.core.view.ViewCompat
import androidx.core.view.WindowInsetsCompat

class MainActivityRoller : AppCompatActivity() {
 override fun onCreate(savedInstanceState: Bundle?) {
 super.onCreate(savedInstanceState)
 enableEdgeToEdge()
 setContentView(R.layout.activity_main_roller)
 ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.main)) { v, insets ->
 val systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars())
 v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom)
 insets
 }
 val backButton = findViewById<Button>(R.id.button_Back)
 backButton.setOnClickListener {
 startActivity(Intent(this, MainActivity::class.java))
 }
 setupDiceButton(R.id.button_d4, 4)
 setupDiceButton(R.id.button_d6, 6)
 setupDiceButton(R.id.button_d8, 8)
 setupDiceButton(R.id.button_d10, 10)
 setupDiceButton(R.id.button_d12, 12)
 setupDiceButton(R.id.button_d20, 20)
 setupDiceButton(R.id.button_d100, 100)
 }

 private fun setupDiceButton(buttonId: Int, diceType: Int) {
 val button = findViewById<Button>(buttonId)
 button.setOnClickListener {
 val intent = Intent(this@MainActivityRoller, Results::class.java).apply {
 putExtra("Dice", diceType)
 }
 startActivity(intent)
 }
 }
}

Resultss.kt

package com.example.diceroller

import android.content.Intent
import android.os.Bundle
import android.widget.Button
import androidx.activity.enableEdgeToEdge
import androidx.appcompat.app.AppCompatActivity
import androidx.core.view.ViewCompat
import androidx.core.view.WindowInsetsCompat
import kotlin.random.Random

class Results : AppCompatActivity() {
    private lateinit var rerollButton: Button

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()
        setContentView(R.layout.activity_results)

        ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.main)) { v, insets ->
            val systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars())
            v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom)
            insets
        }

        rerollButton = findViewById(R.id.rerollButton)

        val diceSize = intent.getIntExtra("Dice", 4)

        rerollButton.setOnClickListener {
            roll(diceSize)
        }
        val backButton = findViewById<Button>(R.id.button_Back2)
        backButton.setOnClickListener {
            startActivity(Intent(this, MainActivityRoller::class.java))
        }

        roll(diceSize)
    }

    private fun roll(d: Int) {
        val res = Random.nextInt(1, d + 1)
        rerollButton.text = res.toString()
    }

}

activity_main.xml

<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <Button
        android:id="@+id/StartButton"
        android:layout_width="match_parent"
        android:layout_height="84dp"
        android:layout_marginStart="30dp"
        android:layout_marginTop="120dp"
        android:layout_marginEnd="30dp"
        android:text="Start"
        android:textSize="34sp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <Button
        android:id="@+id/buttonExit2"
        android:layout_width="match_parent"
        android:layout_height="84dp"
        android:layout_marginStart="30dp"
        android:layout_marginTop="132dp"
        android:layout_marginEnd="30dp"
        android:text="Exit"
        android:textSize="34sp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/StartButton" />

</androidx.constraintlayout.widget.ConstraintLayout>

activity_main_roller.xml

<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivityRoller">

    <Button
        android:id="@+id/button_d4"
        android:layout_width="wrap_content"
        android:layout_height="110dp"
        android:layout_marginStart="48dp"
        android:layout_marginTop="128dp"
        android:text="d4"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <Button
        android:id="@+id/button_d6"
        android:layout_width="wrap_content"
        android:layout_height="110dp"
        android:layout_marginTop="128dp"
        android:layout_marginEnd="48dp"
        android:text="d6"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <Button
        android:id="@+id/button_d8"
        android:layout_width="wrap_content"
        android:layout_height="110dp"
        android:layout_marginStart="48dp"
        android:layout_marginTop="32dp"
        android:text="d8"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/button_d4" />

    <Button
        android:id="@+id/button_d10"
        android:layout_width="wrap_content"
        android:layout_height="110dp"
        android:layout_marginTop="32dp"
        android:layout_marginEnd="48dp"
        android:text="d10"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/button_d6" />

    <Button
        android:id="@+id/button_d12"
        android:layout_width="wrap_content"
        android:layout_height="110dp"
        android:layout_marginStart="48dp"
        android:layout_marginTop="32dp"
        android:text="d12"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/button_d8" />

    <Button
        android:id="@+id/button_d20"
        android:layout_width="wrap_content"
        android:layout_height="110dp"
        android:layout_marginTop="32dp"
        android:layout_marginEnd="48dp"
        android:text="d20"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/button_d10" />

    <Button
        android:id="@+id/button_d100"
        android:layout_width="wrap_content"
        android:layout_height="110dp"
        android:layout_marginStart="48dp"
        android:layout_marginTop="32dp"
        android:text="d100"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/button_d12" />

    <Button
        android:id="@+id/button_Back"
        android:layout_width="wrap_content"
        android:layout_height="110dp"
        android:layout_marginTop="32dp"
        android:layout_marginEnd="48dp"
        android:text="Back"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/button_d20" />
</androidx.constraintlayout.widget.ConstraintLayout>

activity_results.xml

<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".Results">

    <Button
        android:id="@+id/button_Back2"
        android:layout_width="wrap_content"
        android:layout_height="110dp"
        android:layout_marginStart="32dp"
        android:layout_marginEnd="32dp"
        android:layout_marginBottom="32dp"
        android:text="Back"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent" />

    <Button
        android:id="@+id/rerollButton"
        android:layout_width="wrap_content"
        android:layout_height="48pt"
        android:layout_marginStart="128dp"
        android:layout_marginTop="128dp"
        android:layout_marginEnd="128dp"
        android:text="Result"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

</androidx.constraintlayout.widget.ConstraintLayout>

Вывод
В ходе работы было разаработано приложение для ОС Androidб получены навыки работы с ПО Android Studio и Git.
