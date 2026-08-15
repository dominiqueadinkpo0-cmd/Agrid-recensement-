# Audit de rebranding AGRID — Survey Solutions

> Date : 15 août 2026
> Projet : `Agrid-recensement-` (branche `master`) — code source Survey Solutions / World Bank
> Objectif : rebrander « Survey Solutions » en « AGRID » **sans casser le fonctionnement**.

---

## 1. Résumé complet de l'audit

### Structure du dépôt

| Dossier | Rôle |
|---|---|
| `src/UI/WB.UI.Designer` | Webapp de conception (questionnaires) |
| `src/UI/WB.UI.Headquarters.Core` | Interface MVC Headquarters (HQ) |
| `src/UI/WB.UI.Frontend` | SPA Vue (HQ + web interview) |
| `src/UI/WB.UI.WebTester` | Web Tester des questionnaires |
| `src/UI/Interviewer/WB.UI.Interviewer` | App Android Interviewer (Xamarin) |
| `src/UI/Supervisor/WB.UI.Supervisor` | App Android Supervisor (Xamarin) |
| `src/UI/Tester/WB.UI.Tester` | App Android Questionnaire Tester (Xamarin) |
| `src/UI/Shared/WB.UI.Shared.*` | Layouts/ressources Android partagés |
| `src/Core` | Domaines métier (tous bounded contexts) |
| `src/Services/Export` | Microservice d'export (DDI, Stata, etc.) |
| `src/Infrastructure` | Infrastructure (stockage, sérialisation) |
| `src/Tests` | Tests unitaires / web |
| `installer/` | Installeur WiX (bootstrap + MSI + custom actions) |
| `docker/` | Dockerfiles HQ / Designer / WebTester |
| `.github/` | Workflows CI, instructions, templates d'issues |

### Références à la marque (dénombrement)

- « Survey Solutions » : ~270 fichiers (majorité dans les `.resx` de traduction et les locales web)
- « World Bank » : ~33 fichiers (footers, AssemblyInfo, licences, manifests)
- « surveysolutions » (sans espace) : Firebase (`surveysolutions-729db`), chemins de dev générés (`#line`), noms npm
- `org.worldbank` / `worldbank.org` : 26 fichiers (packages Android, User-Agent API, layouts de login)

### Points structurants

- Les textes de marque des **apps mobiles** ne sont pas dans les projets UI Android mais dans les `.resx` des **projets Core** référencés (`WB.Core.SharedKernels.Enumerator`, `WB.Core.BoundedContexts.Tester`, `WB.Enumerator.Native`).
- Les ressources de marque se déclinent en **16 langues** (resx + locales web JSON).
- Le mot « Survey Solutions » apparaît aussi dans des **identifiants techniques** (JWT Issuer, realm d'auth, user-agent d'API, namespaces de sérialisation, noms d'assembly, registre Windows, services/IIS/BD de l'installeur). Ceux-ci ne doivent **pas** être rebrandés (voir §3).

---

## 2. Fichiers de phase 1 — branding visuel (à rebrander)

> Phase 1 = uniquement l'aspect **visible** : textes d'interface et images. Remplacer les **valeurs**, jamais les **clés**.

### Apps web

| Fichier | Contenu à rebrander |
|---|---|
| `src/UI/WB.UI.Designer/Views/Shared/Layout.cshtml` + `Layout.Template.cshtml` | Logo `alt="Survey Solutions"`, lien `mysurvey.solutions` |
| `src/UI/WB.UI.Designer/questionnaire/index.html` | `<title>Designer</title>`, favicons |
| `src/UI/WB.UI.Designer/questionnaire/src/views/Designer/Layout.vue` | Logo `alt="Survey Solutions"`, liens support/forum |
| `src/UI/WB.UI.Designer/questionnaire/src/views/App/components/Header.vue` | Liens support/forum |
| `src/UI/WB.UI.Designer/questionnaire/src/locale/*.json` (17 langues) | Clés `Welcome`, `TesterApp`, `AppDescription`, `CompanyName` |
| `src/UI/WB.UI.Designer/Resources/AccountResources.resx` + locales | Clés `Welcome`, `CompanyName`, `AllRights`, `Legal` |
| `src/UI/WB.UI.Designer/Resources/NotificationResources.resx` + locales | « sent automatically by the Survey Solutions Designer tool » |
| `src/UI/WB.UI.Designer/Resources/QuestionnaireEditor.resx` + locales | « does not appear to come from the Survey Solutions server » |
| `src/UI/WB.UI.Designer/Areas/Pdf/Views/Pdf/*.cshtml` (3) | Logo PDF `suso-logo-dark.svg` |
| `src/UI/WB.UI.Designer/Areas/Identity/Pages/Layout.Account.cshtml` (+Template) | Logos WB |
| `src/UI/WB.UI.Designer/Views/Error/Layout.Error.cshtml` (+Template) | Logos WB |
| `src/UI/WB.UI.Designer/Views/Shared/_CommonFooter.cshtml` | `CompanyName`, `AllRights`, `Legal` (lien Play Store à conserver) |
| `src/UI/WB.UI.Designer/Content/body.css`, `Content/under-construction.css`, `questionnaire/content/designer-start/designer-start.less` | Logos en background |

### Headquarters (web)

| Fichier | Contenu à rebrander |
|---|---|
| `src/UI/WB.UI.Headquarters.Core/Views/Account/LogOn.cshtml`, `LogOn2fa.cshtml`, `LoginWithRecoveryCode.cshtml` | « Survey Solutions Headquarters » |
| `src/UI/WB.UI.Headquarters.Core/Views/WebInterview/_WebInterviewLayout.Template.cshtml` | Wordmark SVG inline (header) + footer |
| `src/UI/WB.UI.Headquarters.Core/Views/WebEmails/EmailHtml.cshtml`, `EmailText.cshtml` | « Powered by Survey Solutions » |
| `src/UI/WB.UI.Headquarters.Core/Views/Shared/Footer.cshtml` | `Layout_WorldBank`, `Layout_RightsReserved`, `Layout_Legal` |
| `src/UI/WB.UI.Headquarters.Core/Resources/Pages*.resx` + locales | Titres de pages (source des `<title>`) |
| `src/UI/WB.UI.Headquarters.Core/Resources/Common*.resx` + locales | `TheWorldBankGroup` |
| `src/UI/WB.UI.Headquarters.Core/Resources/Settings*.resx` + locales | Messages « with Survey Solutions » |
| `src/UI/WB.UI.Headquarters.Core/Resources/Troubleshooting*.resx` + locales | « visit the Survey Solutions support site » |
| `src/UI/WB.UI.Headquarters.Core/Resources/Workspaces*.resx` + locales | « Survey Solutions Interviewer and Supervisor applications » |
| `src/UI/WB.UI.Headquarters.Core/Resources/LoginToDesigner*.resx` + locales | « Survey Solutions Designer credentials » |
| `src/UI/WB.UI.Headquarters.Core/Resources/QuestionnaireImport*.resx` + locales | Titres d'import |
| `src/UI/WB.UI.Headquarters.Core/Resources/SyncLogMessages.cs.resx` + locales | « Garant Survey Solutions » |
| `src/UI/WB.UI.Headquarters.Core/wwwroot/data-export-storages.html` | « Survey Solutions Headquarters website » |

### Frontend SPA

| Fichier | Contenu à rebrander |
|---|---|
| `src/UI/WB.UI.Frontend/index.html` | `<title>Survey Solutions</title>` |
| `src/UI/WB.UI.Frontend/package.json` | `"name": "survey.solutions-frontend"` |
| `src/UI/WB.UI.Frontend/src/hqapp/Views/HQ/Download/Supervisor.vue`, `Interviewer.vue` | « Survey Solutions Headquarters » |
| `src/UI/WB.UI.Frontend/src/hqapp/Views/HQ/WebInterviewSetup/Settings.vue` | « Powered by Survey Solutions » |
| `src/UI/WB.UI.Frontend/src/hqapp/Views/HQ/Template/LoginToDesigner.vue` | Logo Designer, alt |
| `src/UI/WB.UI.Frontend/src/hqapp/Views/HQ/Profile/Profile.vue`, `Assignments/CreateNew.vue`, `Export/Export.vue`, `Admin/EmailProviders.vue` | Liens `support.mysurvey.solutions` |
| `src/UI/WB.UI.Frontend/src/webinterview/components/Navbar.vue` | Lien support |

### Web Tester

| Fichier | Contenu à rebrander |
|---|---|
| `src/UI/WB.UI.WebTester/Views/Error/_ErrorLayout.cshtml` | Wordmark SVG inline |
| `src/UI/WB.UI.WebTester/Resources/Common.resx` + locales | `TheWorldBankGroup`, `Legal` |
| `src/UI/WB.UI.WebTester/Resources/Common.ru.resx` | « Survey Solutions Designer » |

### Apps Android (textes Core)

| Fichier | Contenu à rebrander |
|---|---|
| `src/Core/SharedKernels/Enumerator/Enumerator/Properties/UIResources.resx` + 15 langues | `Interviewer_ApplicationName`, `Supervisor_ApplicationName`, `LoginTitleText`, erreurs |
| `src/Core/SharedKernels/Enumerator/Enumerator/Properties/EnumeratorUIResources.resx` + 15 langues | « Survey Solutions Supervisor », « Survey Solutions server » |
| `src/Core/SharedKernels/Enumerator/WB.Enumerator.Native/Resources/WebInterview.*.resx` (16 langues) | « Survey Solutions Web Survey », page d'accueil |
| `src/Core/BoundedContexts/Tester/WB.Core.BoundedContexts.Tester/Properties/TesterUIResources.resx` + 15 langues | « Survey Solutions Designer », URL login |
| `src/Core/BoundedContexts/Interviewer/.../InterviewerUIResources.cs.resx` | « Garant Survey Solutions » |

### Apps Android (visuel direct)

| Fichier | Contenu à rebrander |
|---|---|
| `src/UI/Interviewer/WB.UI.Interviewer/Properties/AndroidManifest.xml` | `android:label="Interviewer"` → « AGRID Interviewer » |
| `src/UI/Supervisor/WB.UI.Supervisor/Properties/AndroidManifest.xml` | `android:label="Supervisor"` → « AGRID Supervisor » |
| `src/UI/Tester/WB.UI.Tester/Properties/AndroidManifest.xml` | `android:label="Tester"` |
| `src/UI/*/.../Resources/Drawable/capi_splash.png` + `splash.axml` | Splash screens |
| `src/UI/*/.../Resources/drawable-*/icon.png` | Icônes launcher |
| `src/UI/*/.../Resources/drawable-*/login_logo.png` / `loginLogo.png` | Logos de login |

> **Hors périmètre Phase 1B** : `src/UI/*/.../Properties/AssemblyInfo.cs` (`AssemblyCopyright`,
> métadonnées d'assembly, fichiers binaires générés). Ne pas modifier : les informations
> d'attribution existantes sont conservées. *Assembly metadata review postponed; requires
> license review and source verification.*

#### Constats de l'audit Android (15 août 2026 — doc seule, aucun fichier modifié)

| Inventaire | Résultat |
|---|---|
| `AssemblyInfo.cs` | **23** fichiers dans `src/` (Core, UI, Infrastructure, Services, Tests). Encodage hétérogène : **20 en UTF-8 (BOM)**, **3 en UTF-16** (Interviewer, Supervisor, Headquarters.Core). |
| « The World Bank » dans `AssemblyCopyright` | **2 fichiers seulement** : `Interviewer` (« Copyright © The World Bank 2014 ») et `Supervisor` (« Copyright © The World Bank 2018 »), tous deux UTF-16. Les 21 autres ont `AssemblyCopyright("Copyright ©  <année>")` sans mention WB. |
| Manifests Android | `Interviewer` → `android:label="Interviewer"`, package `org.worldbank.solutions.interviewer` ; `Supervisor` → `android:label="Supervisor"`, `org.worldbank.solutions.supervisor` ; `Tester` → `android:label="Tester"`, `org.worldbank.solutions.Vtester`. Les `package` restent **intacts** (contrat Firebase/FileProvider/API — voir §3). |
| Resx apps Core (16 langues × 3 séries) | `UIResources.*.resx`, `EnumeratorUIResources.*.resx`, `TesterUIResources.*.resx` — **50 fichiers** référencent « Survey Solutions » (valeurs à rebrander, clés intactes). |
| Images Android | **15** `icon.png` launcher, **2** `capi_splash.png`, **18** `login_logo.png`/`loginLogo.png` (Interviewer + Supervisor + Tester, densités mdpi→xxxhdpi). |
| Décision Phase 1B | `AssemblyInfo.cs` : **hors périmètre** (attribution WB conservée). Manifests (labels), resx et images : **dans le périmètre**, à traiter en phase applicative. |

### Services d'export

| Fichier | Contenu à rebrander |
|---|---|
| `src/Services/Export/WB.Services.Export.Host/WB.Services.Export.Host.csproj` + `Export/Directory.Build.props` | `<Product>Survey Solutions Export Service</Product>` |
| `src/Services/Export/WB.Services.Export/CsvExport/Implementation/TabularFormatExportService.cs` | « Generated by Survey Solutions export module » |
| `src/Services/Export/WB.Services.Export/ExportProcessHandlers/Externals/OneDriveDataClient.cs`, `GoogleDriveDataClient.cs` | Dossier « Survey Solutions » |
| `src/Services/Export/WB.Services.Export.Tests/CsvExport/Implementation/TabularFormatExportServiceTests.cs` | Asserts (en synchrone) |

### Hors code

| Fichier | Contenu à rebrander |
|---|---|
| `README.md`, `CONTRIBUTING.md`, `SECURITY.md` | Descriptions, URLs |
| `docker/Dockerfile.hq`, `Dockerfile.hq.simple`, `Dockerfile.designer`, `Dockerfile.webtester` | Labels OCI |
| `.github/instructions/*.md`, `.github/copilot-instructions.md` | Descriptions |
| `branding/` | Assets AGRID (déjà ajoutés, source de vérité) |

---

## 3. Fichiers à NE PAS modifier

| Fichier | Raison |
|---|---|
| `LICENSE.md` | World Bank Master Community License Agreement (texte légal) |
| `installer/src/SurveySolutionsProduct/Product.wxs` | Chemins d'installation, registre `SOFTWARE\World Bank\Survey Solutions`, `Database=SurveySolutions`, IIS site/pool, service Windows, DLL custom actions — lié à désinstallation/upgrade |
| `installer/src/SurveySolutionsBootstrap/Bundle.wxs` | Nom de bundle, upgrade, `MsiPackage Id` |
| `installer/src/SurveySolutionsCustomActions/` | `SurveySolutionsCustomActions.CA.dll` référencé par Product.wxs + UnitTest |
| `installer/src/SurveySolutionsProduct/Configuration.wxi` | `SITE_APP_NAME="SurveySolutions"` |
| `src/UI/*/.../Properties/google-services.json` (3) | Firebase `surveysolutions-729db`, clés API, `mobilesdk_app_id` |
| `src/UI/*/.../Properties/AndroidManifest.xml` | Attributs `package="org.worldbank.solutions.*"`, authorities FileProvider |
| `src/UI/Interviewer/.../Resources/xml/file_paths.xml` (+ Supervisor, Tester) | Chemins de fichiers Android |
| `src/UI/WB.UI.Headquarters.Core/Controllers/Api/DataCollection/*` | `ProductName => "org.worldbank.solutions.{interviewer,supervisor}"` (contrat User-Agent) |
| `src/UI/WB.UI.Headquarters.Core/Code/Extensions.cs` | Regex `org\.worldbank\.solutions\.(?<appname>…)` |
| `src/UI/WB.UI.Headquarters.Core/appsettings.ini` | `[JwtBearer] Issuer=Survey.Solutions` — renommer invalide les tokens existants |
| `src/UI/WB.UI.Designer/Startup.cs` | `opts.Realm = "mysurvey.solutions"` — auth |
| `src/UI/WB.UI.Designer/Code/HistoryExtensions.cs` | Détection de domaine `.mysurvey.solutions` — logique métier |
| `src/UI/WB.UI.Headquarters.Core/Controllers/UsersController.cs` | Issuer 2FA `"Survey Solutions"` — renommer invalide les 2FA configurés |
| `src/UI/WB.UI.Frontend/vite.config.js` | Pipeline de build (ne toucher ni aux entrées ni aux cibles) |
| `src/Infrastructure/WB.Infrastructure.Native/Storage/NewToOldAssemblyRedirectSerializationBinder.cs` | Types persistés en base |
| Namespaces `WB.Core.SharedKernels.SurveySolutions.*` (≈60 fichiers) | Sérialisation/redirection d'assembly |
| `src/Services/Export/Utils/ddidotnet/*`, `Utils/StatData/*` | `Copyright © Sergiy Radyakin, The World Bank 2015` (attribution tiers) |
| `.github/workflows/ci.yml` | Clés SonarCloud `surveysolutions_surveysolutions` |
| `.build.ps1` | Tags DockerHub `surveysolutions/surveysolutions` (registre public) |
| Toutes les URLs `*.mysurvey.solutions`, `support@mysurvey.solutions`, `github.com/surveysolutions/surveysolutions` | Endpoints et liens fonctionnels |
| Clés de ressources `.resx` / `locale/*.json` | Ne changer que les **valeurs**, jamais les **clés** (build de traduction cassé) |
| `$id` du schéma `docs.mysurvey.solutions/schemas/questionnaire.schema.json` | Contrat de document exporté |
| Sélecteurs E2E `data-suso="…"`, classes `suso-*` | Tests automatisés |
| Noms d'assembly / namespaces `WB.*` | Build .NET, `Directory.Build.targets`, `@using` des cshtml |
| `google_maps_api_key`, `arcgisruntime_key`, Crashlytics `build_id` | Clés/build injectés au build |
| `DesignerEndpoint` (`designer.mysurvey.solutions`) | Endpoint serveur fonctionnel |

---

## 4. Risques identifiés

| Niveau | Risque | Détail |
|---|---|---|
| **HAUT** | Auth et tokens | `Issuer=Survey.Solutions` (JWT), realm `mysurvey.solutions`, issuer 2FA → toute modification invalide tokens/sessions/2FA existants |
| **HAUT** | Packages Android | `org.worldbank.solutions.*` liés à Firebase, FileProvider, user-agents API → casse crash reporting et compatibilité serveur |
| **HAUT** | Installeur WiX | Chemins `C:\Survey Solutions`, registre, services/IIS/BD → casse désinstallation, upgrade, données existantes |
| **HAUT** | Sérialisation | Namespaces `SurveySolutions.*` persistés en base → perte d'accès aux données si renommés |
| **MOYEN** | Traductions | 16+ langues à maintenir en synchrone (resx + locales JSON) |
| **MOYEN** | Exports | Textes « Survey Solutions » dans les fichiers exportés + tests associés à mettre à jour ensemble |
| **MOYEN** | Chemins d'assets en dur | Renommer une image sans mettre à jour toutes ses références (CSS, cshtml, `vite.config.js`) → page blanche |
| **MOYEN** | Build front | `vite.config.js` régénère cshtml et locales ; toute incohérence casse le build |
| **FAIBLE** | Cosmétique | Logos, alt, titres, valeurs resx → aucun impact fonctionnel |

---

## 5. Stratégie recommandée pour AGRID

### Principe
Rebrander **l'aspect visible** uniquement, en conservant l'identité technique intacte : packages, contrats d'API, auth, sérialisation, installeur.

### Étapes

1. **Préparer les images dérivées** (machine avec PIL/ImageMagick) depuis `branding/` :
   - favicons 72/76/114/120/144/152/180/192 (+ `favicon.ico`, `favicon-hq*.png`, `favicon-designer*.png`)
   - icônes Android `icon.png` mdpi(48)/hdpi(72)/xhdpi(96)/xxhdpi(144)
   - `login_logo.png` + `capi_splash.png` (splash)
   - logos web `logo.png` / `logo.svg`, `designer-logo.png`, `headquarter_logo*.png`, `logo-retina.png`
   - remplacer les fichiers existants **en gardant les noms identiques**

2. **Web — textes** : remplacer les **valeurs** des clés resx / locale JSON (16 langues) : `Welcome`, `AppDescription`, `CompanyName`, `PageTitle`, `TesterApp`, `TheWorldBankGroup`, messages d'erreur. Jamais les clés.

3. **Web — vues** : éditer les `.cshtml` / `.vue` de la liste phase 1 (titres, alt, footers, wordmarks SVG inline).

4. **Android** : `android:label` des 3 manifests + valeurs des `UIResources.resx` / `EnumeratorUIResources.resx` / `TesterUIResources.resx`. `AssemblyInfo.cs` **hors périmètre** (attribution conservée — voir §2).

5. **Services** : `<Product>` des csproj Export, textes des exports (+ tests en synchrone).

6. **Hors code** : README, SECURITY, Docker labels, `.github/instructions` — libre et sans risque.

7. **Vérifier** : build .NET (`WB.sln`), build front (`vite.config.js`), tests export ; contrôle que **rien dans §3 n'a été touché**.

### Hors périmètre (ne pas faire)
- Installeur WiX (phase ultérieure avec upgrade/registre si souhaité)
- Issuer JWT / realm / 2FA
- Packages Android / Firebase
- Namespaces de sérialisation
- URLs `*.mysurvey.solutions` (endpoints réels)

---

## 6. Références

- Assets AGRID : `branding/` (logo, transparent, icône app, login, favicon — thème navy/gold/white/light green)
- Audit complet des apps web, Android et hors-code : réalisé le 15 août 2026 sur le dépôt cloné

---

## 7. Matrice détaillée Phase 1B — Android (50 resx + 35 images)

> Phase 1B = uniquement les valeurs visibles. **Aucun fichier modifié lors de cet audit** :
> matrice de référence pour la phase applicative. Règle d'or : ne changer que les **valeurs**,
> jamais les **clés** (`<data name="…">` intact).

### 7.1 Règles de remplacement (applicables à toutes les langues)

| Motif ancien | Remplacé par AGRID |
|---|---|
| `Survey Solutions` (générique) | `AGRID` |
| `Survey Solutions Interviewer` | `AGRID Interviewer` |
| `Survey Solutions Supervisor` | `AGRID Supervisor` |
| `Survey Solutions Designer` | `AGRID Designer` |
| `Survey Solutions server` | `AGRID server` |
| `Survey Solutions Web Survey` | `AGRID Web Survey` |
| `Survey Solutions Questionnaire Tester` | `AGRID Questionnaire Tester` |
| `Garant Survey Solutions` (cs) | `Garant AGRID` |

> Dans les traductions (ar, cs, es, fr, id, ka, km, pt, ro, ru, sq, th, uk, vi, zh), la marque
> est translittérée en alphabet local : remplacer la séquence `Survey Solutions` par `AGRID`
> **en gardant la structure grammaticale locale** (ex. fr : « Enquêteur Survey Solutions » →
> « Enquêteur AGRID » ; ru : « Survey Solutions Designer » → « AGRID Designer »). Les clés et
> les `\n`, espaces et `{0}` de format restent inchangés.

### 7.2 Séries de fichiers resx — cartographie applicative

| Série | App concernée | Projet | Fichiers |
|---|---|---|---|
| `UIResources.*.resx` | **Shared** (Interviewer + Supervisor + Tester) | `Core/SharedKernels/Enumerator/Enumerator/Properties` | 16 |
| `EnumeratorUIResources.*.resx` | **Shared** (Interviewer + Supervisor) | `Core/SharedKernels/Enumerator/Enumerator/Properties` | 16 |
| `WebInterview.*.resx` | **Web Survey** (page d'enquête web partagée) | `Core/SharedKernels/Enumerator/WB.Enumerator.Native/Resources` | 16 |
| `TesterUIResources.*.resx` | **Tester** (app Tester / Designer) | `Core/BoundedContexts/Tester/WB.Core.BoundedContexts.Tester/Properties` | 16 (dont 2 avec branding) |

### 7.3 Matrice resx — fichiers et clés (valeurs base EN)

> Ancienne valeur = base (`UIResources.resx` / `EnumeratorUIResources.resx` / `WebInterview.resx`,
> langue EN). Nouvelle valeur = proposition AGRID. Les traductions suivent la règle §7.1.

#### `UIResources.*.resx` — Shared (Interviewer/Supervisor/Tester) — 16 fichiers

| Clé | Ancienne valeur (base EN) | Nouvelle valeur AGRID | Action |
|---|---|---|---|
| `LoginTitleText` | Survey Solutions Questionnaire Tester | AGRID Questionnaire Tester | modifier |
| `Interviewer_ApplicationName` | Survey Solutions Interviewer | AGRID Interviewer | modifier |
| `Supervisor_ApplicationName` | Survey Solutions Supervisor | AGRID Supervisor | modifier |
| `ErrorMessage_Maintenance` | The website of Survey Solutions Designer is on the maintenance mode now. Sorry for the inconvenience | The website of AGRID Designer is on the maintenance mode now. Sorry for the inconvenience | modifier |
| `ErrorMessage_RequestTimeout` | Timeout when connecting to the Survey Solutions Designer website. Check your internet connection. | Timeout when connecting to the AGRID Designer website. Check your internet connection. | modifier |
| `ErrorMessage_ServiceUnavailable` | No connection to the Survey Solutions Designer. Please make sure that the website is available. | No connection to the AGRID Designer. Please make sure that the website is available. | modifier |
| `ErrorMessage_InvalidEndpoint` | Provided Survey Solutions Designer URL is invalid. Check settings. | Provided AGRID Designer URL is invalid. Check settings. | modifier |
| `Dashboard_EmptyQuestionnairesList` | No questionnaires yet. \n\nYou can create or edit your questionnaires in Survey Solutions Designer | No questionnaires yet. \n\nYou can create or edit your questionnaires in AGRID Designer | modifier |
| `MissingPermissions_Storage_Global` | To store maps on the mobile device Survey Solutions needs access to the files. … | To store maps on the mobile device AGRID needs access to the files. … | modifier |

> Clés en plus selon langue : `ErrorMessage_UpgradeRequired` (th), `LoginTitleText` (ru = « Survey
> Solutions Questionnaire Tester » → « AGRID Questionnaire Tester »). Fichiers : base + `ar, cs, es,
> fr, id, ka, km, pt, ro, ru, sq, th, uk, vi, zh` (le `.ar` ne référence que
> `MissingPermissions_Storage_Global`).

#### `EnumeratorUIResources.*.resx` — Shared (Interviewer/Supervisor) — 16 fichiers

| Clé | Ancienne valeur (base EN) | Nouvelle valeur AGRID | Action |
|---|---|---|---|
| `Maintenance` | The website of Survey Solutions Supervisor is on maintenance. Sorry for the inconvenience | The website of AGRID Supervisor is on maintenance. Sorry for the inconvenience | modifier |
| `RequestTimeout` | Timeout when connecting to the Survey Solutions Supervisor website. Check your internet connection. | Timeout when connecting to the AGRID Supervisor website. Check your internet connection. | modifier |
| `ServiceUnavailable` | No connection to the Survey Solutions Supervisor. Please make sure that the website is available. | No connection to the AGRID Supervisor. Please make sure that the website is available. | modifier |
| `CommunicationIntegrityFailed` | The response received does not appear to come from the Survey Solutions server. … | The response received does not appear to come from the AGRID server. … | modifier |
| `Prefs_AllowSyncWithHq_Summary_Disabled` (cs) | … aplikací Garant Survey Solutions | … aplikací Garant AGRID | modifier |
| `ApplicationIncompatibleWithServer`, `InvalidEndpoint`, `Prefs_BufferSizeSummary`, `Prefs_EndpointTitle`, `Troubleshooting_NoNewVersion`, `UpgradeRequired` (th) | « Survey Solutions Interviewer » / « Survey Solutions Supervisor » | « AGRID Interviewer » / « AGRID Supervisor » | modifier |

#### `WebInterview.*.resx` — Web Survey — 16 fichiers

| Clé | Ancienne valeur (base EN) | Nouvelle valeur AGRID | Action |
|---|---|---|---|
| `WelcomeText` | Hello and Welcome to the Survey Solutions Web Survey | Hello and Welcome to the AGRID Web Survey | modifier |
| `WebSurvey` | Survey Solutions Web Survey | AGRID Web Survey | modifier |
| `Resume_WelcomeText` | Hello and Welcome to the Survey Solutions Web Survey | Hello and Welcome to the AGRID Web Survey | modifier |

> Les **16 fichiers** de la série (base + 15 langues) référencent les 3 clés. Traductions : la
> séquence `Survey Solutions` est remplacée par `AGRID` dans la phrase locale.

#### `TesterUIResources.*.resx` — Tester — 2 fichiers avec branding (sur 16)

| Fichier | Clé | Ancienne valeur | Nouvelle valeur AGRID | Action |
|---|---|---|---|---|
| `TesterUIResources.ru.resx` | `Prefs_DesignerEndPointSummary` | URL для подключения к приложению Survey Solutions Designer | URL для подключения к приложению AGRID Designer | modifier |
| `TesterUIResources.ru.resx` | `ImportQuestionnaire_Error_PreconditionFailed` | … Исправьте, пожалуйста, их в Survey Solutions Designer. | … Исправьте, пожалуйста, их в AGRID Designer. | modifier |
| `TesterUIResources.th.resx` | `ImportQuestionnaire_Error_PreconditionFailed` | … กรุณาแก้ไขแบบสอบถามที่ Survey Solutions Designer | … กรุณาแก้ไขแบบสอบถามที่ AGRID Designer | modifier |
| `TesterUIResources.th.resx` | `Login_Error_NotFound` | … ของ Survey Solutions Designer … | … ของ AGRID Designer … | modifier |
| `TesterUIResources.th.resx` | `Prefs_DesignerEndPointSummary` | URL ที่ใช้ในการติดต่อกับ Survey Solutions Designer | URL ที่ใช้ในการติดต่อกับ AGRID Designer | modifier |
| `TesterUIResources.th.resx` | `Prefs_DesignerEndPointTitle` | URL ของ Survey Solutions Designer | URL ของ AGRID Designer | modifier |

> **Conserver** : le fichier base `TesterUIResources.resx` et les 13 autres langues (aucune
> référence à la marque — clés `…Designer…` génériques déjà compatibles AGRID).

### 7.4 Matrice images — 35 fichiers

> Rôles : **icône** (launcher), **splash** (capi_splash), **login** (logo d'écran de connexion).
> Asset source AGRID : `branding/agrid-app-icon-512.png` (icône), `branding/agrid-login-logo-800.png`
> (login/splash), `branding/agrid-logo-transparent.png` (splash alt). Redimensionner en gardant le
> **nom et le chemin exacts** de chaque fichier cible.

#### Interviewer — 12 fichiers (`src/UI/Interviewer/WB.UI.Interviewer/Resources/`)

| Chemin | Rôle | Dimensions actuelles | Asset AGRID source (redimensionné) |
|---|---|---|---|
| `Drawable/Icon.png` | icône | 512×512 | `agrid-app-icon-512.png` (512) |
| `Drawable/capi_splash.png` | splash | 480×800 | `agrid-logo-transparent.png` composé sur fond (480×800) |
| `Drawable/login_logo.png` | login | 800×800 | `agrid-login-logo-800.png` (800) |
| `drawable-mdpi/icon.png` | icône | 48×48 | `agrid-app-icon-512.png` (48) |
| `drawable-hdpi/icon.png` | icône | 72×72 | `agrid-app-icon-512.png` (72) |
| `drawable-xhdpi/icon.png` | icône | 96×96 | `agrid-app-icon-512.png` (96) |
| `drawable-xxhdpi/icon.png` | icône | 144×144 | `agrid-app-icon-512.png` (144) |
| `drawable-mdpi/login_logo.png` | login | 240×240 | `agrid-login-logo-800.png` (240) |
| `drawable-hdpi/login_logo.png` | login | 360×360 | `agrid-login-logo-800.png` (360) |
| `drawable-xhdpi/login_logo.png` | login | 480×480 | `agrid-login-logo-800.png` (480) |
| `drawable-xxhdpi/login_logo.png` | login | 720×720 | `agrid-login-logo-800.png` (720) |
| `drawable-xxxhdpi/login_logo.png` | login | 960×960 | `agrid-login-logo-800.png` (960) |

#### Supervisor — 12 fichiers (`src/UI/Supervisor/WB.UI.Supervisor/Resources/`)

| Chemin | Rôle | Dimensions actuelles | Asset AGRID source (redimensionné) |
|---|---|---|---|
| `Drawable/Icon.png` | icône | 512×512 | `agrid-app-icon-512.png` (512) |
| `Drawable/capi_splash.png` | splash | 480×800 | `agrid-logo-transparent.png` composé sur fond (480×800) |
| `Drawable/login_logo.png` | login | 800×800 | `agrid-login-logo-800.png` (800) |
| `drawable-mdpi/icon.png` | icône | 48×48 | `agrid-app-icon-512.png` (48) |
| `drawable-hdpi/icon.png` | icône | 72×72 | `agrid-app-icon-512.png` (72) |
| `drawable-xhdpi/icon.png` | icône | 96×96 | `agrid-app-icon-512.png` (96) |
| `drawable-xxhdpi/icon.png` | icône | 144×144 | `agrid-app-icon-512.png` (144) |
| `drawable-mdpi/login_logo.png` | login | 240×240 | `agrid-login-logo-800.png` (240) |
| `drawable-hdpi/login_logo.png` | login | 360×360 | `agrid-login-logo-800.png` (360) |
| `drawable-xhdpi/login_logo.png` | login | 480×480 | `agrid-login-logo-800.png` (480) |
| `drawable-xxhdpi/login_logo.png` | login | 720×720 | `agrid-login-logo-800.png` (720) |
| `drawable-xxxhdpi/login_logo.png` | login | 960×960 | `agrid-login-logo-800.png` (960) |

#### Tester — 11 fichiers (`src/UI/Tester/WB.UI.Tester/Resources/`)

| Chemin | Rôle | Dimensions actuelles | Asset AGRID source (redimensionné) |
|---|---|---|---|
| `Drawable/Icon.png` | icône | 512×512 | `agrid-app-icon-512.png` (512) |
| `Drawable/loginLogo.png` | login | 800×800 | `agrid-login-logo-800.png` (800) |
| `drawable-mdpi/icon.png` | icône | 48×48 | `agrid-app-icon-512.png` (48) |
| `drawable-hdpi/icon.png` | icône | 72×72 | `agrid-app-icon-512.png` (72) |
| `drawable-xhdpi/icon.png` | icône | 96×96 | `agrid-app-icon-512.png` (96) |
| `drawable-xxhdpi/icon.png` | icône | 144×144 | `agrid-app-icon-512.png` (144) |
| `drawable-mdpi/loginLogo.png` | login | 240×240 | `agrid-login-logo-800.png` (240) |
| `drawable-hdpi/loginLogo.png` | login | 360×360 | `agrid-login-logo-800.png` (360) |
| `drawable-xhdpi/loginLogo.png` | login | 480×480 | `agrid-login-logo-800.png` (480) |
| `drawable-xxhdpi/loginLogo.png` | login | 720×720 | `agrid-login-logo-800.png` (720) |
| `drawable-xxxhdpi/loginLogo.png` | login | 960×960 | `agrid-login-logo-800.png` (960) |

### 7.5 Ordre recommandé d'application des changements Android (Phase 1B)

1. **Préparer les assets** (machine avec ImageMagick/PIL, hors repo) : redimensionner
   `agrid-app-icon-512.png` → 7 tailles d'icône (48/72/96/144/512), `agrid-login-logo-800.png` →
   6 tailles de login (240/360/480/720/800/960), préparer le splash 480×800 à partir de
   `agrid-logo-transparent.png`. Ne rien committer tant que les visuels ne sont pas validés.
2. **Manifests** : `android:label` des 3 `AndroidManifest.xml`
   (`Interviewer` → « AGRID Interviewer », `Supervisor` → « AGRID Supervisor », `Tester` →
   « AGRID Tester »). Ne **pas** toucher à `package=` (contrat Firebase/API — §3).
3. **Textes partagés** : `UIResources.*.resx` (16) puis `EnumeratorUIResources.*.resx` (16) —
   appliquer §7.1, d'abord la base EN, puis les 15 traductions.
4. **Textes Web Survey** : `WebInterview.*.resx` (16) — les 3 clés.
5. **Textes Tester** : `TesterUIResources.ru.resx` et `.th.resx` (2 fichiers, 6 clés).
6. **Images** : remplacer les 35 fichiers par les assets redimensionnés **en conservant noms et
   chemins** (Interviewer 12, Supervisor 12, Tester 11).
7. **Conserver** : `AssemblyInfo.cs` (23) — hors périmètre, attribution WB intacte.
8. **Vérifier** : `git status` → seuls les fichiers resx/manifests/images attendus changent ;
   build .NET (`WB.sln`) + contrôle qu'aucun fichier §3 n'a été touché ; test unitaire resx
   (build de traduction) puis validation visuelle sur émulateur/device.