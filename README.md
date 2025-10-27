# Sessió: Formularis en PHP amb Docker (2 hores)

> **Objectiu general**: entendre i practicar l’enviament de dades amb **GET**, **POST**, **pujada de fitxers**, **validació al servidor**, **escapat de sortides** i **protecció CSRF** en PHP, utilitzant un entorn Docker llest per a classe.

---

## 1) Objectius d’aprenentatge (al final de la sessió l’alumnat podrà…)
- Distingir quan emprar **GET** vs **POST** i llegir `$_GET`, `$_POST` i `$_FILES`.
- Implementar **validacions bàsiques** al servidor (camps obligatoris, formats, longituds).
- Fer una **pujada de fitxers** segura (tipus i mida) amb `enctype="multipart/form-data"`.
- Aplicar **escapat de sortides** amb `htmlspecialchars` per evitar XSS.
- Explicar i demostrar un **token CSRF** i per què és necessari.
- Posar en marxa un mini projecte **PHP 8 + Apache** amb **Docker** i `docker compose`.

---

## 2) Materials i prerequisits
- Docker Desktop instal·lat (o Docker Engine) i `docker compose`.
- Un editor de codi (VS Code, IntelliJ/PHPStorm, etc.).
- Projecte base **“php-forms-docker”** (a l’annex tens tots els fitxers).
- Navegador web modern.
- (Opcional) projector per mostrar el debug en directe.

---

## 3) Preparació prèvia (docent)
1. Copia la carpeta **php-forms-docker** al teu equip (estructura a l’Annex A).
2. Obre un terminal a la carpeta del projecte i arrenca:
   ```bash
   docker compose up --build
   ```
3. Comprova que s’obre `http://localhost:8080` i que la pàgina mostra els tres formularis.
4. (Opcional) Prepara algun fitxer de prova (PDF i imatges) per a la pujada.

---

## 4) Agenda (120 minuts)
**0:00–0:10** — Context i objectius + repàs molt breu HTTP i formularis  
**0:10–0:25** — GET: paràmetres a l’URL i lectura amb `$_GET`  
**0:25–0:50** — POST + validació al servidor (nom, email, missatge)  
**0:50–1:10** — Pujada de fitxers: `enctype`, límits de mida i tipus  
**1:10–1:25** — CSRF + sessions i `htmlspecialchars` (evitar XSS)  
**1:25–1:50** — Pràctica guiada per parelles (mini-reptes)  
**1:50–2:00** — Wrap‑up + *exit ticket* i deures opcionals

> **Notes didàctiques**: alterna explicació curta + demostració en viu + comprovació ràpida (preguntes de 30s). Mantén el ritme amb temporitzador visible.

---

## 5) Guió pas a pas (amb notes per al docent)

### Bloc 1 — Context (0:00–0:10)
- **Explica**: HTTP, formularis, ruta (*endpoint*), mètode (**GET/POST**), cos i capçaleres.
- **Mostra** la pàgina d’exemple (index) i on veure `$_GET`, `$_POST` i `$_FILES` al panell *Debug*.

### Bloc 2 — GET (0:10–0:25)
- **Demostra** la cerca (formulari GET). Escriu una paraula i destaca com apareix `?q=...` a l’URL.
- **Fes notar**: GET és idoni per cerques, filtres i enllaços compartibles. No apte per dades sensibles.
- **Mini-pregunta**: On es troben les dades d’un GET al servidor? (Resposta: `$_GET`).

### Bloc 3 — POST + validació (0:25–0:50)
- **Omple** el formulari de contacte amb dades incorrectes: mostra missatges d’error.
- **Explica** validació al servidor: *mai* confiar només en el client.
- **Fes un “happy path”** i mostra el missatge d’èxit.
- **Subratlla** l’ús d’`htmlspecialchars` al pintar dades d’usuari.

### Bloc 4 — Upload (0:50–1:10)
- **Demostra** la pujada d’un PDF i d’una imatge (correcte) i després un tipus no permès.
- **Explica**: `enctype="multipart/form-data"`, limit de **2 MB**, comprovació d’**extensió** i **MIME**.
- **Mostra** la carpeta `src/uploads` i el nom segur generat.

### Bloc 5 — CSRF + XSS (1:10–1:25)
- **Explica**: sessió, token CSRF a un `input hidden`, i validació amb `hash_equals`.
- **Prova** a enviar un POST sense token (forçant) i mostra l’error CSRF.
- **XSS**: ensenya com `htmlspecialchars` evita injectar HTML/JS quan es mostren dades.

### Bloc 6 — Pràctica guiada (1:25–1:50)
Treball per parelles (10–15 min) + repàs ràpid (10 min):
1. **GET**: afegeix un segon paràmetre (ex.: `cat=php`) i mostra’l sota la caixa de cerca.
2. **POST**: afegeix camp “**assumpte**” amb validació (mínim 3 caràcters).
3. **UPLOAD**: puja només **PNG** i **PDF** (retira JPG/JPEG) i limita a **1 MB**.
4. **CSRF**: canvia el token de sessió i comprova que el POST falla.
5. **XSS**: intenta escriure `<script>alert(1)</script>` en el missatge i verifica que no s’executa.

> **Solucions/Expectatives**: a l’annex tens fragments de codi orientatius dins `index.php`.

### Bloc 7 — Wrap‑up (1:50–2:00)
- **Exit ticket (1–2 min)**: respon en un *post-it* o al xat del LMS:
  - Quan faig servir GET vs POST?
  - On es defineix l’`enctype` per a pujar fitxers?
  - Per què necessito un token CSRF?
- **Deures opcionals**: escriu un formulari de registre amb camps addicionals i pujada d’avatar (PNG ≤ 1 MB), i mostra tots els errors sota els camps.

---

## 6) Avaluació formativa (checklist ràpid)
- [ ] Diferencia correctament GET i POST i sap on llegir `$_GET`/`$_POST`.
- [ ] Implementa validacions bàsiques al servidor (obligatorietat, format, longitud).
- [ ] Configura pujada de fitxers amb `enctype` i limita mida/tipus.
- [ ] Escapa sortides amb `htmlspecialchars`.
- [ ] Explica el flux del token CSRF (generació, enviament, validació).
- [ ] Sap arrencar el projecte amb Docker i fer proves al navegador.

---

## 7) Troubleshooting (freqüents)
- **No veig la pàgina a `localhost:8080`** → comprova que el contenidor està “healthy” i que el port no està ocupat; reinicia amb `docker compose down && docker compose up --build`.
- **Error de permisos en `uploads`** → crea la carpeta i dona permisos en entorns estrictes:
  ```bash
  mkdir -p src/uploads && chmod 777 src/uploads
  ```
- **El POST diu “Token CSRF invàlid”** → recarrega la pàgina (nova sessió/token) i envia de nou.
- **El fitxer pujat es rebutja** → revisa extensió i MIME; prova amb un `.pdf` o `.png` petit.

---

## Annex A — Projecte “php-forms-docker” (codi complet)

### A.1 — Estructura de carpetes
```
php-forms-docker/
├─ docker-compose.yml
├─ Dockerfile
└─ src/
   └─ index.php
```

### A.2 — `docker-compose.yml`
```yaml
version: "3.9"
services:
  web:
    build: .
    ports:
      - "8080:80"
    volumes:
      - ./src:/var/www/html
    restart: unless-stopped
```

### A.3 — `Dockerfile`
```dockerfile
FROM php:8.3-apache

# Activa mod_rewrite (no imprescindible per a l'exemple, però útil en projectes reals)
RUN a2enmod rewrite

WORKDIR /var/www/html
```

### A.4 — `src/index.php`
```php
<?php
// --- Sessió per al CSRF ---
session_start();

// Helpers senzills
function e($s) { return htmlspecialchars($s ?? '', ENT_QUOTES | ENT_SUBSTITUTE, 'UTF-8'); }

function csrf_token() {
    if (empty($_SESSION['csrf'])) {
        $_SESSION['csrf'] = bin2hex(random_bytes(32));
    }
    return $_SESSION['csrf'];
}

function valid_csrf($token) {
    return isset($_SESSION['csrf']) && hash_equals($_SESSION['csrf'], $token ?? '');
}

$errors = [];
$flash  = [];

// --- Tractament de POST (contacte i upload) ---
if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    $action = $_POST['action'] ?? '';

    if (!valid_csrf($_POST['csrf'] ?? '')) {
        $errors['csrf'] = 'Token CSRF invàlid o inexistent. Torna-ho a provar.';
    } else {
        if ($action === 'contact') {
            $name    = trim($_POST['name'] ?? '');
            $email   = trim($_POST['email'] ?? '');
            $subject = trim($_POST['subject'] ?? ''); // camp afegit per a la pràctica guiada
            $message = trim($_POST['message'] ?? '');

            if ($name === '' || mb_strlen($name) < 2) {
                $errors['name'] = 'El nom és obligatori (mínim 2 caràcters).';
            }
            if (!filter_var($email, FILTER_VALIDATE_EMAIL)) {
                $errors['email'] = 'El correu no és vàlid.';
            }
            if ($subject !== '' && mb_strlen($subject) < 3) {
                $errors['subject'] = 'L\'assumpte ha de tenir com a mínim 3 caràcters.';
            }
            if ($message === '' || mb_strlen($message) < 5) {
                $errors['message'] = 'El missatge és obligatori (mínim 5 caràcters).';
            }

            if (!$errors) {
                // Aquí podries enviar correu, guardar a BD, etc.
                $flash[] = 'Formulari de contacte rebut correctament!';
            }
        }

        if ($action === 'upload' && !$errors) {
            if (!isset($_FILES['document']) || $_FILES['document']['error'] !== UPLOAD_ERR_OK) {
                $errors['document'] = 'Puja un fitxer vàlid.';
            } else {
                $file     = $_FILES['document'];
                $maxBytes = 2 * 1024 * 1024; // 2 MB (modifica a 1 * 1024 * 1024 per a la pràctica)
                if ($file['size'] > $maxBytes) {
                    $errors['document'] = 'El fitxer supera el límit de 2MB.';
                } else {
                    // Comprovació bàsica de tipus per extensió i MIME
                    $allowedExts  = ['pdf','png','jpg','jpeg']; // per a la pràctica, prova amb ['pdf','png']
                    $allowedMimes = ['application/pdf','image/png','image/jpeg'];

                    $ext = strtolower(pathinfo($file['name'], PATHINFO_EXTENSION));
                    $f   = new finfo(FILEINFO_MIME_TYPE);
                    $mime = $f->file($file['tmp_name']);

                    if (!in_array($ext, $allowedExts, true) || !in_array($mime, $allowedMimes, true)) {
                        $errors['document'] = 'Tipus de fitxer no permès. (Admesos: pdf, png, jpg)';
                    } else {
                        $uploadDir = __DIR__ . '/uploads';
                        if (!is_dir($uploadDir)) {
                            @mkdir($uploadDir, 0777, true);
                        }
                        $safeName = preg_replace('/[^a-zA-Z0-9._-]/','_', basename($file['name']));
                        $target   = $uploadDir . '/' . time() . '_' . $safeName;
                        if (move_uploaded_file($file['tmp_name'], $target)) {
                            $url = 'uploads/' . basename($target);
                            $flash[] = 'Fitxer pujat correctament: <a href="' . e($url) . '" target="_blank">' . e($url) . '</a>';
                        } else {
                            $errors['document'] = 'No s’ha pogut moure el fitxer.';
                        }
                    }
                }
            }
        }
    }
}

// --- Tractament GET (exemple de cerca) ---
$search = trim($_GET['q'] ?? '');
$cat    = trim($_GET['cat'] ?? ''); // paràmetre extra per a la pràctica guiada

?>
<!doctype html>
<html lang="ca">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Demo Formularis PHP (GET, POST, Upload, CSRF)</title>
  <style>
    body { font-family: system-ui, -apple-system, Segoe UI, Roboto, Ubuntu, Cantarell, 'Helvetica Neue', Arial, sans-serif; margin: 2rem; }
    h1 { margin-bottom: .25rem; }
    .grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(320px, 1fr)); gap: 1.5rem; }
    form { border: 1px solid #ddd; padding: 1rem; border-radius: .75rem; }
    label { display:block; margin-top:.5rem; font-weight: 600; }
    input[type=text], input[type=email], textarea, input[type=file] { width: 100%; padding: .5rem; margin-top: .25rem; border:1px solid #ccc; border-radius:.5rem; }
    button { margin-top: .75rem; padding:.6rem 1rem; border:0; border-radius:.6rem; cursor:pointer; }
    .ok { background:#e6ffed; border:1px solid #b7f5ca; padding:.5rem .75rem; border-radius:.5rem; }
    .err { color:#b00020; font-size:.95rem; }
    details { margin-top: 1rem; }
    code, pre { background:#f6f8fa; padding:.2rem .4rem; border-radius:.25rem; }
  </style>
</head>
<body>
  <h1>Demo de Formularis en PHP</h1>
  <p>Aquesta pàgina mostra com funcionen <strong>GET</strong>, <strong>POST</strong> i <strong>pujada d’arxius</strong>, amb <strong>validació</strong> i <strong>CSRF</strong>.</p>

  <?php if ($flash): ?>
    <div class="ok">
      <?php foreach ($flash as $m): ?>
        <div><?= $m ?></div>
      <?php endforeach; ?>
    </div>
  <?php endif; ?>

  <div class="grid">
    <!-- Formulari GET: Cerca -->
    <form method="get">
      <h2>Cerca (mètode GET)</h2>
      <p>Els paràmetres es veuen a l’URL (ex.: <code>?q=...</code>).</p>
      <label for="q">Què vols cercar?</label>
      <input type="text" id="q" name="q" value="<?= e($search) ?>" placeholder="Ex.: php forms" />

      <label for="cat">Categoria (opcional)</label>
      <input type="text" id="cat" name="cat" value="<?= e($cat) ?>" placeholder="Ex.: php" />

      <button type="submit">Cerca (GET)</button>

      <?php if ($search !== '' || $cat !== ''): ?>
        <p>Has cercat: <strong><?= e($search) ?></strong><?php if ($cat !== ''): ?> (categoria: <strong><?= e($cat) ?></strong>)<?php endif; ?></p>
      <?php endif; ?>
    </form>

    <!-- Formulari POST: Contacte amb validació -->
    <form method="post" novalidate>
      <h2>Contacte (mètode POST)</h2>
      <input type="hidden" name="action" value="contact">
      <input type="hidden" name="csrf" value="<?= e(csrf_token()) ?>">

      <label for="name">Nom</label>
      <input type="text" id="name" name="name" value="<?= e($_POST['name'] ?? '') ?>">
      <?php if (isset($errors['name'])): ?><div class="err"><?= e($errors['name']) ?></div><?php endif; ?>

      <label for="email">Correu</label>
      <input type="email" id="email" name="email" value="<?= e($_POST['email'] ?? '') ?>">
      <?php if (isset($errors['email'])): ?><div class="err"><?= e($errors['email']) ?></div><?php endif; ?>

      <label for="subject">Assumpte (opcional)</label>
      <input type="text" id="subject" name="subject" value="<?= e($_POST['subject'] ?? '') ?>">
      <?php if (isset($errors['subject'])): ?><div class="err"><?= e($errors['subject']) ?></div><?php endif; ?>

      <label for="message">Missatge</label>
      <textarea id="message" name="message" rows="4"><?= e($_POST['message'] ?? '') ?></textarea>
      <?php if (isset($errors['message'])): ?><div class="err"><?= e($errors['message']) ?></div><?php endif; ?>

      <?php if (isset($errors['csrf']) && ($_POST['action'] ?? '') === 'contact'): ?><div class="err"><?= e($errors['csrf']) ?></div><?php endif; ?>

      <button type="submit">Envia (POST)</button>
    </form>

    <!-- Formulari POST: Upload d'arxius -->
    <form method="post" enctype="multipart/form-data">
      <h2>Pujada d’arxius</h2>
      <p>Admesos: <code>.pdf</code>, <code>.png</code>, <code>.jpg</code> (fins a 2MB).</p>
      <input type="hidden" name="action" value="upload">
      <input type="hidden" name="csrf" value="<?= e(csrf_token()) ?>">

      <label for="document">Selecciona fitxer</label>
      <input type="file" id="document" name="document" accept=".pdf,.png,.jpg,.jpeg">
      <?php if (isset($errors['document'])): ?><div class="err"><?= e($errors['document']) ?></div><?php endif; ?>
      <?php if (isset($errors['csrf']) && ($_POST['action'] ?? '') === 'upload'): ?><div class="err"><?= e($errors['csrf']) ?></div><?php endif; ?>

      <button type="submit">Puja fitxer</button>
    </form>
  </div>

  <details>
    <summary>Debug de superglobals</summary>
    <pre><strong>$_GET</strong>  = <?php var_export($_GET); ?></pre>
    <pre><strong>$_POST</strong> = <?php $postCopy = $_POST; unset($postCopy['csrf']); var_export($postCopy); ?></pre>
    <pre><strong>$_FILES</strong> = <?php var_export($_FILES); ?></pre>
  </details>

  <hr>
  <p><small>Consells didàctics: recorda <code>method="get"</code> vs <code>method="post"</code>, l’<code>enctype="multipart/form-data"</code> per a uploads, l’ús de <code>$_GET</code>, <code>$_POST</code>, <code>$_FILES</code>, i sempre escapar amb <code>htmlspecialchars</code> per evitar XSS. El token CSRF evita enviaments cross-site.</small></p>
</body>
</html>
```

---

## Annex B — Comandes útils
```bash
# Arrencar el projecte
docker compose up --build

# Parar i netejar contenidors
docker compose down

# Crear carpeta d'uploads i donar permisos (si cal)
mkdir -p src/uploads && chmod 777 src/uploads
```

---

## Annex C — Mini‑cheatsheet (PHP)
- `$_GET['nom_camp']` → llegeix un camp d’un formulari GET
- `$_POST['nom_camp']` → llegeix un camp d’un formulari POST
- `$_FILES['document']` → info del fitxer pujat (nom, mida, tipus…) quan `enctype="multipart/form-data"`
- `htmlspecialchars($value, ENT_QUOTES | ENT_SUBSTITUTE, 'UTF-8')` → escapat segur
- `session_start()` → inicia sessió (necessari per CSRF i *flash messages*)
- `hash_equals($a, $b)` → comparació segura de tokens CSRF
- `move_uploaded_file($tmp, $dest)` → mou l’upload a destí
- `finfo(FILEINFO_MIME_TYPE)` → obté el MIME real d’un fitxer

---

**Llicència i ús**: pots reutilitzar aquest material lliurement a classe.
