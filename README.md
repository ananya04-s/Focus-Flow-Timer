⏱️ FocusFlow Timer
A premium, minimalist productivity timer app built in Kotlin for Android, featuring a glassmorphism UI, smooth animations, custom circular progress ring, Stopwatch & Pomodoro modes, Pause/Resume, and a live Statistics card.

🎯 Project Overview
FocusFlow Timer is a single-screen Android app designed to help users stay focused and productive. It teaches core Android concepts — Handler-based timing, ViewModel for configuration-change survival, LiveData observation, ViewBinding, and custom View drawing — wrapped in a polished, portfolio-ready interface.

✨ Features

Stopwatch Mode — counts up from 00:00:00, one ring rotation per hour.
Pomodoro Mode — counts down from 25:00, ring empties as time elapses.
▶ Start / ⏸ Pause / ▶ Resume / ↺ Reset — full timer lifecycle with no duplicate starts.
Custom Circular Progress Ring — drawn on Canvas in TimerRingView; sky-blue when idle, emerald-green while running.
Glassmorphism Timer Card — frosted semi-transparent card with a glowing border over the midnight-blue gradient background.
Motivational Quotes — rotate every 10 seconds while the timer is running ("Keep Going! 🚀", "Stay in the zone! 🔥", and more).
Statistics Card — displays current Session time, Best session (persisted across resets), and total Starts for the session.
Dark Mode — full Material 3 dark color scheme via values-night/colors.xml.
Rotation-safe — TimerViewModel survives screen rotation via AndroidX ViewModel.
Splash Screen — branded midnight-blue splash using the AndroidX SplashScreen API.
Smooth Animations — fade-in on launch, button-press scale, pulse ring animation.


🛠️ Technologies Used
LayerTechnologyLanguageKotlinIDEAndroid StudioUIXML Layouts, Material Design 3, ViewBindingTimerHandler.postDelayed (1 s tick)StateViewModel + LiveDataCustom UICanvas-drawn TimerRingViewAnimationsXML <set> anims + View.startAnimation()Min SDK24 · Target/Compile SDK 34

🖼️ Screenshots

Add screenshots of the Splash, Idle state, Running state (emerald ring), and Pomodoro mode here.

SplashIdleRunningPomodoroscreenshotscreenshotscreenshotscreenshot

📂 Folder Structure
app/src/main/java/com/example/focusflow/
├── FocusFlowApp.kt              # Application class
├── ui/
│   ├── SplashActivity.kt        # 1.5 s branded splash
│   ├── MainActivity.kt          # Timer screen, button logic, LiveData observers
│   └── TimerRingView.kt         # Custom Canvas circular progress ring
├── timer/
│   ├── TimerState.kt            # Enums: TimerState, TimerMode, constants
│   ├── TimerManager.kt          # Handler-based tick engine
│   └── TimerViewModel.kt        # ViewModel wrapping TimerManager + stats
└── util/
    └── FormatUtils.kt           # Seconds → HH:MM:SS formatter

app/src/main/res/
├── layout/
│   ├── activity_splash.xml
│   └── activity_main.xml
├── drawable/                    # gradient BGs, glass cards, button BGs, icon
├── anim/                        # fade_in, pulse, btn_press
├── values/ & values-night/      # colors, strings, themes
└── xml/                         # backup & data extraction rules

⚙️ How the Timer Works
1. Handler tick — TimerManager schedules itself every 1 second:
kotlinprivate val tickRunnable = object : Runnable {
    override fun run() {
        if (_state.value != TimerState.RUNNING) return
        _elapsedSeconds.value = (_elapsedSeconds.value ?: 0L) + 1L
        handler.postDelayed(this, 1000L)
    }
}
2. Prevent duplicate starts — a guard flag blocks re-entry:
kotlinfun start() {
    if (_state.value == TimerState.RUNNING) return  // guard
    _state.value = TimerState.RUNNING
    if (!isScheduled) { isScheduled = true; handler.postDelayed(tickRunnable, 1000L) }
}
3. Reset — removes the pending callback immediately:
kotlinfun reset() {
    handler.removeCallbacks(tickRunnable)
    isScheduled = false
    _state.value = TimerState.IDLE
    _elapsedSeconds.value = 0L   // display returns to 00:00:00
}
4. Survive rotation — TimerViewModel extends ViewModel, so Android keeps it alive across Activity re-creation. The Handler continues ticking while MainActivity re-binds its LiveData observers.

🚀 Setup Instructions

Extract the zip and open the FocusFlow folder in Android Studio Hedgehog (2023.1.1) or newer.
Let Gradle sync (first run requires internet to download dependencies).
Connect a device or start an emulator running Android 8.0 (API 24)+.
Click Run ▶.
Tap ▶ Start to begin counting.
Tap ⏸ Pause to pause without losing progress.
Tap ↺ Reset to stop and return to 00:00:00.
Switch to the 🍅 Pomodoro chip for a 25-minute countdown.


🔮 Future Enhancements

Sound notification / vibration when Pomodoro session ends.
Custom countdown duration input.
Session history stored in Room database.
Daily productivity heatmap / streak tracker.
Lottie animations (confetti on Pomodoro completion).
Background service so the timer keeps running when minimized.
Home screen widget.


👤 Author
FocusFlow Timer — built as a portfolio/internship project demonstrating Handler-based timing, ViewModel + LiveData, custom Canvas drawing, and Material Design 3 in Kotlin.
