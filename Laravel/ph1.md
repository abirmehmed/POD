## 📘 **Lecture 1.1 – Course Overview & What is Laravel?**  
**Professor Dr. Alistair Finch**  
*Duration: ~35 minutes (part of Day 1, first sub‑lecture)*  

---

### [0:00] Opening – Why you’re here

**Finch:**  
*“Alright, let’s begin. Close your browsers – except the terminal. I’ll wait.”*

You signed up for a 45‑class journey from **zero Laravel to a production SaaS app**. That’s not a promise – it’s a guarantee, **if** you do the work.  

Today is **Day 1, Sub‑lecture 1.1**: Course Overview and What is Laravel?  

By the end of this 35‑minute block, you will:  
- Know exactly what Laravel is and why it dominates PHP development.  
- Understand the course structure, grading, and expectations.  
- Be able to answer the question *“Why Laravel over raw PHP or other frameworks?”* with real arguments.  

**No theory without purpose.** Every concept I teach today will be used in the next 30 minutes.

---

### [5:00] What is Laravel? – The 60‑second answer

Laravel is a **free, open‑source PHP web framework** created by Taylor Otwell in 2011. It follows the **Model‑View‑Controller (MVC)** architectural pattern.  

**What does that mean in plain English?**  
It gives you a set of tools and rules to build web applications **faster**, **safer**, and **more maintainably** than writing raw PHP.

**Key features you’ll use within the first week:**  
- **Routing** – clean URL handling.  
- **Blade** – a templating engine that compiles to plain PHP.  
- **Eloquent ORM** – talk to databases using PHP objects, not SQL strings.  
- **Artisan** – a command‑line tool that generates code, runs migrations, and serves your app.  
- **Authentication** – login, registration, password reset – built in.  

**Why do companies pay Laravel developers $80k‑$150k?**  
Because Laravel turns a 2‑week project into a 3‑day project, safely.  

---

### [10:00] Deep dive: Laravel vs. raw PHP (with a live example)

Let me show you the difference.  

**Raw PHP – fetching a user from a database:**  
```php
// raw.php
$pdo = new PDO('mysql:host=localhost;dbname=test', 'user', 'pass');
$stmt = $pdo->prepare("SELECT * FROM users WHERE id = ?");
$stmt->execute([$_GET['id']]);
$user = $stmt->fetch();
echo "<h1>" . htmlspecialchars($user['name']) . "</h1>";
```

That works. But it’s **fragile** – SQL injection if you forget `prepare`, no separation of concerns, hard to test.

**Laravel – same thing:**  
```php
// routes/web.php
Route::get('/user/{id}', function ($id) {
    $user = User::find($id);
    return view('profile', ['user' => $user]);
});
```

And the view `profile.blade.php`:  
```html
<h1>{{ $user->name }}</h1>
```

**What did Laravel give you for free?**  
- Automatic CSRF protection.  
- SQL injection prevention (Eloquent uses PDO binding).  
- A clean separation: route → controller → model → view.  
- You can test that route with one line of PHPUnit.

**This is not magic – it’s convention over configuration.** Laravel assumes you’ll organise your code a certain way, and rewards you with less boilerplate.

---

### [18:00] Course structure – Your roadmap

Look at the syllabus (I’m projecting it now).  

**Five phases, 45 classes, ~116 hours total.**

| Phase | Days | Focus | Output |
|-------|------|-------|--------|
| 1 | 1‑7 | Foundation: MVC, routing, Blade, Eloquent basics | Static blog with dynamic routes |
| 2 | 8‑14 | Core: Auth, CRUD, validation, file uploads | Multi‑user blog platform |
| 3 | 15‑24 | Intermediate: APIs, testing, caching, mail | RESTful task manager API |
| 4 | 25‑35 | Advanced: Queues, broadcasting, Livewire, performance | Real‑time chat room |
| 5 | 36‑45 | Capstone: SaaS with Stripe, roles, CI/CD, deployment | Production SaaS app |

**Each class is 2‑3 hours.** We will have:  
- A warm‑up recap (15 min)  
- Two deep‑dive sessions (40‑50 min each) with live coding  
- Two Q&A blocks (10 min each)  
- A common mistakes section (15 min)  
- A hands‑on exercise (20‑30 min)  
- A small test (10 min)  
- Homework assignment (5 min)  

**Grading:**  
- 40% – Four mini‑projects (one per phase, except capstone)  
- 20% – Weekly quizzes (short, practical)  
- 30% – Final capstone SaaS project  
- 10% – Participation & attendance  

**I don’t grade on memorisation. I grade on working code.**

---

### [25:00] Why Laravel? – The pragmatic argument

**Student question I always get:** *“Why not Django, Rails, or Node.js?”*

Here’s my answer:  

- **PHP runs 78% of the web.** Shared hosting, cheap servers, WordPress – the ecosystem is massive. Laravel is the premium choice within PHP.  
- **Laravel’s learning curve is gentle.** Compared to Symfony (steep) or Django (full‑stack opinionated), Laravel lets you start simple and add complexity as you grow.  
- **Job market:** Search “Laravel developer” on any job board. Hundreds of openings. Many are remote.  
- **Ecosystem:** Cashier (Stripe), Spark (SaaS boilerplate), Nova (admin panel), Forge (server management) – Laravel is a full solution, not just a framework.

**But the real reason:** Laravel respects your time. You can build a prototype in one day and a production app in one week. That’s not hyperbole – I’ve done it 20+ times.

---

### [30:00] Q&A Block 1 (5 min)

**Student:** *“Do I need to know OOP before starting?”*  
**Finch:** *“Basic OOP – classes, objects, methods, properties – yes. Advanced patterns like dependency injection? No, we’ll teach that. If you’re shaky on OOP, review PHP.net’s OOP tutorial before Day 3.”*

**Student:** *“I’ve used CodeIgniter before. How different is Laravel?”*  
**Finch:** *“Very different. Laravel uses modern PHP (8.2+), has a robust ORM, and follows more strict MVC. You’ll find Laravel more expressive but with a steeper initial setup. Stick with it – after one week you won’t want to go back.”*

**Student:** *“What if I miss a class?”*  
**Finch:** *“Every lecture is recorded (if live online) and all code is on GitHub. But missing two in a row is dangerous – you’ll fall behind. Communicate with me. I’m strict but fair.”*

---

### [33:00] Small test (to be done after this sub‑lecture)

*“Write your answers on a paper or text file. We’ll review in 2 minutes.”*

1. Name three built‑in features Laravel provides out of the box.  
2. What does MVC stand for? (We’ll cover the details in 1.2, but you should know the acronym now.)  
3. How many classes total in this course?  
4. What percentage of your grade is the final capstone project?  
5. Write the command to create a new Laravel project called `my_app` (just the command – assume Composer is installed).

**Answers:**  
1. Routing, Blade templating, Eloquent ORM, Artisan, Authentication, etc. (any three).  
2. Model‑View‑Controller.  
3. 45 classes.  
4. 30%.  
5. `composer create-project laravel/laravel my_app`

---

### [34:30] Common misconceptions about Laravel (quick list)

| Misconception | Truth |
|---------------|-------|
| “Laravel is slow.” | Raw PHP is faster, but with caching (config, route, view) and Octane, Laravel handles thousands of requests per second. Don’t optimise prematurely. |
| “Laravel is only for small projects.” | Laravel powers big apps like Invoice Ninja, Laracasts, and many enterprise SaaS products. |
| “I need to learn every Laravel feature to be productive.” | No. You can build 80% of apps with routing, Eloquent, Blade, and Artisan. The rest you learn on demand. |

---

### [35:00] Homework for 1.1 (to be done before 1.2)

**Finch:**  
*“Before our next sub‑lecture (MVC Architecture), do these three things:”*

1. **Install PHP 8.2+ and Composer** if you haven’t already. Follow the commands I’ll provide in the handout.  
2. **Run** `composer create-project laravel/laravel test-app` – just to see it work. You can delete it after.  
3. **Read** the first page of Laravel’s “Why Laravel” documentation (I’ll link it).  
4. **Write down one question** about Laravel you want answered. Bring it to the next class.

---

**End of Lecture 1.1 – Course Overview & What is Laravel.**  

*“See you in 15 minutes for 1.2 – MVC Architecture Deep Dive.”*
