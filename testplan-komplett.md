# E-Commerce Shop Relaunch — Kompletter Testplan

> Ziel: Vollstaendige Auflistung aller testbaren Bereiche, Seiten, Funktionen und Szenarien.
> Status: Komplett — 53 Kategorien, 727 Testpunkte (eigenes Wissen + Online-Recherche)

---

## 1. Startseite / Homepage

- Hero-Banner / Slider Anzeige
- Kategorien-Uebersicht / Navigation
- Produkt-Highlights / Bestseller
- Aktuelle Angebote / Sale-Banner
- Suchfeld Anzeige + Funktionalitaet
- Newsletter-Anmeldung
- Trust-Elemente (Siegel, Bewertungen, USPs)
- Footer (Links, Kontakt, Social Media, Zahlungsarten-Icons)
- Cookie-Banner / Consent-Management
- Ladezeit / Performance
- Above-the-fold Inhalte korrekt
- Responsive Darstellung (Mobile, Tablet, Desktop)
- SEO Meta-Tags (Title, Description, OG-Tags)
- Strukturierte Daten (Schema.org)
- Breadcrumbs
- Sprachumschaltung (falls mehrsprachig)

## 2. Navigation / Header

- Hauptmenue alle Links funktionieren
- Mega-Menu / Dropdown-Menues
- Mobile Hamburger-Menu
- Sticky Header Verhalten beim Scrollen
- Logo verlinkt auf Startseite
- Warenkorb-Icon mit Zaehler
- Wunschliste-Icon
- Account-Icon / Login-Link
- Suchfeld im Header
- Kategorien-Baum komplett navigierbar
- Breadcrumb-Navigation auf allen Seiten

## 3. Kategorieseiten

- Kategorie-Uebersicht Darstellung
- Unterkategorien Navigation
- Produktliste korrekte Anzeige
- Produktbilder laden korrekt
- Preisanzeige (inkl. UVP (RRP), Rabatt, Grundpreis (unit price))
- Sortierung (Preis aufsteigend/absteigend, Beliebtheit, Neueste, Name, Bewertung)
- Filterung (Preis-Range, Farbe, Groesse, Material, Marke, Bewertung)
- Filter-Kombinationen (mehrere Filter gleichzeitig)
- Filter zuruecksetzen
- Pagination / Infinite Scroll
- Produkte pro Seite (Anzahl aendern)
- Rasteransicht / Listenansicht umschalten
- Leere Kategorie Darstellung
- Kategorie-Beschreibungstext (SEO)
- Kategorie-Bild / Banner
- Quick-View / Schnellansicht
- Zur Wunschliste hinzufuegen
- In den Warenkorb direkt von Kategorieseite

## 4. Produktseite

- Produktbilder (Galerie, Zoom, Lightbox)
- Bilderwechsel (Thumbnails, Slider)
- Produktname korrekt
- Preis Anzeige (Einzelpreis, Staffelpreise (tiered pricing), Grundpreis (unit price))
- MwSt-Hinweis (VAT notice)
- Versandkosten-Hinweis
- Verfuegbarkeitsstatus (auf Lager, wenig auf Lager, nicht verfuegbar)
- Lieferzeit-Anzeige
- Produktbeschreibung (kurz + lang)
- Technische Daten / Spezifikationen
- Produktvarianten (Groesse, Farbe, Material etc.)
- Variantenwechsel aktualisiert Preis
- Variantenwechsel aktualisiert Bild
- Variantenwechsel aktualisiert Verfuegbarkeit
- Mengenauswahl (Quantity Selector)
- Mengen-Limits (Min/Max)
- In den Warenkorb Button
- Sofort kaufen Button (falls vorhanden)
- Wunschliste Button
- Produktvergleich
- Bewertungen / Reviews Anzeige
- Bewertung abgeben
- Bewertung mit Bildern
- Bewertungsdurchschnitt / Sterne
- Frage zum Produkt stellen
- Social Sharing Buttons
- Tabs (Beschreibung, Bewertungen, FAQ, Lieferung)
- Aehnliche Produkte / Cross-Selling
- Zubehoer / Upselling
- Zuletzt angesehene Produkte
- Breadcrumbs
- Strukturierte Daten (Product Schema, Bewertungen)
- Canonical URL
- SEO Meta-Tags

## 5. Produktoptionen / Konfigurator

- Einfache Optionen (Dropdown, Radio, Checkbox)
- Pflichtoptionen vs. optionale Optionen (required vs. optional)
- Optionen mit Preisaufschlag
- Optionen mit Preisabschlag
- Prozentuale Preisaenderungen
- Farbauswahl (Swatches)
- Groessenauswahl
- Materialauswahl
- Textfeld-Eingabe (Gravur, Personalisierung)
- Datei-Upload (eigenes Design)
- Konfiguratorlogik (Abhaengigkeiten zwischen Optionen)
- Nicht verfuegbare Kombinationen ausgegraut
- Preisaktualisierung in Echtzeit
- Mengenrabatte / Staffelpreise
- Bundle-Produkte / Sets
- Gewichtsberechnung bei Optionswechsel
- Vorschau-Bild bei Optionswechsel

## 6. Suche

- Suchfeld Eingabe + Abschicken
- Autocomplete / Suggest
- Suchergebnisse Anzeige
- Suchergebnisse Sortierung
- Suchergebnisse Filterung
- Keine Ergebnisse Darstellung
- Tippfehler-Toleranz (Fuzzy Search)
- Synonyme
- Suche nach Artikelnummer / SKU
- Suche nach Kategorie
- Suche nach Marke
- Suchbegriff-Highlighting in Ergebnissen
- Suchhistorie
- Populaere Suchbegriffe
- Leere Suche abfangen
- Sonderzeichen in Suche
- XSS-Schutz bei Sucheingabe

## 7. Warenkorb

- Produkt hinzufuegen (von Produktseite)
- Produkt hinzufuegen (von Kategorieseite)
- Warenkorb-Anzeige (Mini-Cart / Sidebar)
- Warenkorb-Seite vollstaendig
- Menge aendern
- Menge auf 0 setzen / Produkt entfernen
- Einzelprodukt entfernen
- Warenkorb leeren
- Preisberechnung korrekt (Einzelpreis x Menge)
- Zwischensumme korrekt
- MwSt-Berechnung und Anzeige (VAT calculation)
- Versandkosten-Vorschau
- Gutscheincode eingeben
- Gutscheincode entfernen
- Mehrere Gutscheine (falls erlaubt)
- Gutschein-Validierung (abgelaufen, Mindestbestellwert, bereits verwendet)
- Cross-Selling im Warenkorb
- Warenkorb-Persistenz (nach Browser schliessen)
- Warenkorb-Merge (Gast → Login)
- Leerer Warenkorb Darstellung
- Weiter einkaufen Button
- Zur Kasse Button
- Gewicht-/Massanzeige
- Lieferzeit-Anzeige pro Artikel
- Nicht verfuegbare Artikel im Warenkorb markieren
- Maximalbestellmenge pro Produkt
- Mindestbestellwert (minimum order value)

## 8. Checkout / Kasse

### 8.1 Adresseingabe
- Rechnungsadresse Formular (alle Felder)
- Lieferadresse abweichend
- Adressvalidierung (PLZ-Format, Pflichtfelder)
- Autovervollstaendigung Adresse
- Firmenadresse (Firmenname, USt-ID (VAT ID))
- Packstation / Postfiliale (parcel locker / post office)
- Laenderauswahl
- Gespeicherte Adressen (eingeloggter Kunde)
- Gastbestellung (ohne Account)

### 8.2 Versandoptionen
- Versandarten Anzeige (Standard, Express, Abholung)
- Versandkosten korrekt berechnet
- Versandkostenfrei ab Betrag X (free shipping threshold)
- Lieferzeit-Anzeige pro Versandart
- Wunschliefertermin (falls vorhanden)
- Sperrgut-Zuschlag (bulky goods surcharge)
- Insel-Zuschlag (island surcharge)
- Internationale Versandkosten
- Click & Collect / Abholung im Laden

### 8.3 Zahlungsarten
- Kreditkarte (Visa, Mastercard, Amex)
- PayPal
- PayPal Express
- Klarna (Rechnung, Ratenkauf, Sofort)
- Amazon Pay
- Apple Pay
- Google Pay
- SEPA Lastschrift (SEPA direct debit)
- Vorkasse / Ueberweisung (prepayment / bank transfer)
- Nachnahme (cash on delivery)
- Kauf auf Rechnung (purchase on invoice)
- Ratenzahlung (installment payments)
- Zahlungsart-Auswahl UI
- Zahlungsart-Validierung
- 3D Secure / SCA
- Fehlgeschlagene Zahlung (Handling + Fehlermeldung)
- Zahlungs-Timeout
- Doppelte Zahlung verhindern

### 8.4 Bestelluebersicht
- Alle Positionen korrekt aufgelistet
- Preise korrekt
- Versandkosten angezeigt
- MwSt aufgeschluesselt (VAT breakdown)
- Gutschein-Rabatt angezeigt
- Gesamtbetrag korrekt
- AGB Checkbox (Terms & Conditions)
- Widerrufsbelehrung Checkbox (cancellation policy)
- Datenschutz Checkbox
- Bestellung absenden Button
- Double-Submit Protection

### 8.5 Bestellbestaetigung
- Thank-You-Seite Anzeige
- Bestellnummer angezeigt
- Zusammenfassung der Bestellung
- Bestaetigunsmail versendet
- Bestaetigunsmail Inhalt korrekt
- Weiterleitung nach Zahlung (Redirect URLs)
- Tracking-Pixel / Conversion-Tracking

## 9. Express Checkout

- Apple Pay Button auf Produktseite
- Google Pay Button auf Produktseite
- PayPal Express Button auf Produktseite
- Express Checkout im Warenkorb
- Express Checkout auf Checkout-Seite
- Adressuebertragung aus Wallet
- Versandkosten-Berechnung im Express-Flow
- Abbruch Express Checkout → zurueck zum normalen Flow
- Payment Intent Erstellung
- Payment Intent Update bei Aenderungen

## 10. Kundenkonto / Account

### 10.1 Registrierung
- Registrierungsformular
- Pflichtfelder-Validierung
- E-Mail-Format-Validierung
- Passwort-Staerke-Anforderungen
- Passwort-Wiederholung
- Datenschutz-Checkbox
- Newsletter-Opt-in
- Double-Opt-in E-Mail
- Bestaetigungs-Link funktioniert
- Bereits registrierte E-Mail abfangen
- Social Login (Google, Facebook, Apple)
- Registrierung waehrend Checkout

### 10.2 Login
- Login mit E-Mail + Passwort
- Login mit Username (falls unterstuetzt)
- Falsche Credentials Fehlermeldung
- Account gesperrt nach X Fehlversuchen
- "Passwort vergessen" Flow
- Passwort-Reset E-Mail
- Passwort-Reset Link Ablauf
- Neues Passwort setzen
- "Angemeldet bleiben" / Remember Me
- Session-Timeout
- Login Redirect (zurueck zur vorherigen Seite)
- Social Login
- Zwei-Faktor-Authentifizierung (falls vorhanden)

### 10.3 Kundenkonto-Bereich
- Dashboard / Uebersicht
- Persoenliche Daten aendern
- E-Mail-Adresse aendern
- Passwort aendern
- Adressbuch (Adressen hinzufuegen, bearbeiten, loeschen)
- Standardadresse festlegen
- Bestellhistorie / Bestelluebersicht
- Einzelne Bestellung anzeigen (Details)
- Bestellung erneut bestellen
- Rechnung herunterladen (PDF)
- Sendungsverfolgung / Tracking (shipment tracking)
- Retoure einleiten (initiate return)
- Wunschliste anzeigen
- Newsletter-Einstellungen
- Account loeschen (delete account — DSGVO (GDPR))
- Abmelden / Logout

## 11. Wunschliste / Wishlist

- Produkt zur Wunschliste hinzufuegen
- Produkt von Wunschliste entfernen
- Wunschliste anzeigen
- Von Wunschliste in Warenkorb
- Wunschliste teilen (Link, E-Mail)
- Wunschliste als Gast (Cookie-basiert)
- Wunschliste als eingeloggter User (persistent)
- Wunschliste nach Login uebernehmen

## 12. Bewertungen / Reviews

- Bewertung abgeben (Sterne + Text)
- Bewertung mit Bild/Video
- Bewertung nur fuer verifizierte Kaeufer
- Bewertungs-Moderation (nicht sofort sichtbar)
- Bewertung bearbeiten
- Bewertung loeschen
- Bewertungen sortieren (neueste, beste, schlechteste)
- Bewertungen filtern (nach Sternen)
- Durchschnittsbewertung korrekt berechnet
- Bewertungen auf Produktseite
- Bewertungen auf Kategorieseite (Sterne)
- Bewertungs-Schema (Rich Snippets)

## 13. Gutscheine / Rabatte / Promotions

- Prozent-Gutschein
- Festbetrag-Gutschein
- Versandkostenfrei-Gutschein
- Mindestbestellwert (minimum order value)-Gutschein
- Einmal-Gutschein vs. Mehrfach-Gutschein
- Gutschein pro Kunde limitiert
- Gutschein zeitlich begrenzt (Ablaufdatum)
- Gutschein fuer bestimmte Produkte/Kategorien
- Gutschein-Stapelung (mehrere gleichzeitig)
- Automatische Rabatte (z.B. ab 3 Stueck 10%)
- Mengenrabatte / Staffelpreise (volume discounts / tiered pricing)
- Sale-Preise / Durchgestrichene Preise (sale prices / strikethrough prices)
- Bundle-Rabatte
- Neukundenrabatt (new customer discount)
- Treuepunkte / Bonusprogramm (loyalty points / rewards program)
- Geschenkgutscheine (gift certificates — kaufen + einloesen)

## 14. CMS / Inhaltsseiten

- Impressum (legal notice / imprint)
- Datenschutzerklaerung (privacy policy)
- AGB (Terms & Conditions)
- Widerrufsbelehrung (cancellation policy)
- Versandinformationen (shipping information)
- Zahlungsarten-Seite
- FAQ / Haeufige Fragen
- Ueber uns
- Kontaktseite
- Kontaktformular (Validierung + Versand)
- Blog / Ratgeber
- Blog-Einzelseite
- Blog-Kategorien
- Blog-Suche
- Landingpages
- 404-Fehlerseite
- Wartungsseite

## 15. Kontaktmoeglichkeiten

- Kontaktformular Validierung
- Kontaktformular Versand
- Kontaktformular Bestaetigungsmail
- Telefonnummer klickbar (Mobile)
- E-Mail-Adresse klickbar
- Live-Chat Widget
- WhatsApp Button
- Social-Media-Links
- FAQ / Self-Service

## 16. E-Mails (Transaktional)

- Registrierungsbestaetigung
- Double-Opt-in E-Mail
- Passwort-Reset E-Mail
- Bestellbestaetigung
- Zahlungsbestaetigung
- Versandbestaetigung mit Tracking-Link
- Lieferbestaetigung
- Rechnungs-E-Mail (mit PDF)
- Bewertungsanfrage (nach Lieferung)
- Warenkorb-Erinnerung (Abandoned Cart)
- Wunschliste-Erinnerung
- Newsletter-Willkommenmail
- Newsletter-Abmeldebestaetigung
- Gutschein-E-Mail
- Retoure-Bestaetigung
- Erstattungs-Bestaetigung
- Account-Loeschung Bestaetigung
- E-Mail-Darstellung (Desktop, Mobile, Dark Mode)
- E-Mail-Links funktionieren
- E-Mail Absender / Reply-To korrekt
- SPF/DKIM/DMARC korrekt
- Spam-Score

## 17. SEO

- Title-Tags alle Seiten
- Meta-Descriptions alle Seiten
- H1-Struktur (genau eine pro Seite)
- Heading-Hierarchie (H1 > H2 > H3)
- Canonical URLs
- Hreflang Tags (mehrsprachig)
- XML-Sitemap vorhanden + korrekt
- Robots.txt korrekt
- 301-Redirects von alter Seite
- Keine 404-Fehler bei wichtigen URLs
- Bild-Alt-Texte
- Interne Verlinkung
- Breadcrumbs
- Strukturierte Daten (Organization, Product, Review, FAQ, Breadcrumb)
- Open Graph Tags
- Twitter Cards
- Pagespeed / Core Web Vitals (LCP, FID, CLS)
- Mobile-Friendliness
- URL-Struktur (sprechende URLs)
- Trailing Slashes konsistent
- Index/Noindex korrekt gesetzt
- Paginierung (rel=next/prev oder Alternativen)

## 18. Performance / Ladezeiten

- Startseite Ladezeit
- Kategorieseite Ladezeit
- Produktseite Ladezeit
- Checkout Ladezeit
- Time to First Byte (TTFB)
- Largest Contentful Paint (LCP)
- First Input Delay (FID) / Interaction to Next Paint (INP)
- Cumulative Layout Shift (CLS)
- Bildoptimierung (WebP, AVIF, Lazy Loading)
- CSS/JS Minifizierung
- HTTP/2 oder HTTP/3
- Gzip/Brotli Komprimierung
- Browser-Caching korrekt
- CDN funktioniert
- Datenbankabfragen optimiert
- Server-Response-Zeit
- Ladezeit unter Last (Concurrent Users)
- Third-Party-Scripts Auswirkung

## 19. Mobile / Responsive

- Alle Seiten auf Mobile korrekt
- Alle Seiten auf Tablet korrekt
- Touch-Targets gross genug (min. 44x44px)
- Swipe-Gesten (Bildergalerie, Slider)
- Mobile Navigation (Hamburger Menu)
- Mobile Checkout optimiert
- Mobile Payment (Apple Pay, Google Pay)
- Landscape + Portrait Modus
- Pinch-to-Zoom auf Produktbildern
- Sticky Add-to-Cart auf Mobile
- Mobile Tastatur (numerisch bei PLZ, E-Mail-Tastatur bei E-Mail)
- Formulare auf Mobile nutzbar
- Popups auf Mobile schliessbar
- Viewport Meta-Tag korrekt

## 20. Cross-Browser Kompatibilitaet

- Chrome (Desktop + Mobile)
- Firefox (Desktop + Mobile)
- Safari (Desktop + Mobile / iOS)
- Edge (Desktop)
- Samsung Internet (Mobile)
- Opera (optional)
- Verschiedene OS-Versionen (iOS 15+, Android 10+)
- Verschiedene Bildschirmaufloesungen

## 21. Barrierefreiheit / Accessibility (a11y)

- Tastaturnavigation (Tab, Enter, Escape)
- Focus-Styles sichtbar
- Screenreader-kompatibel (ARIA Labels)
- Alt-Texte auf allen Bildern
- Farbkontraste ausreichend (WCAG AA)
- Schriftgroesse anpassbar
- Formularlabels korrekt verknuepft
- Fehlermeldungen screenreader-zugaenglich
- Skip-to-Content Link
- Keine rein visuellen Informationen (z.B. nur Farbe = Status)
- Video-Untertitel (falls Videos vorhanden)

## 22. Sicherheit / Security

- HTTPS auf allen Seiten
- HSTS Header
- Content Security Policy
- XSS-Schutz (Eingabefelder, URL-Parameter)
- SQL-Injection Schutz
- CSRF-Schutz (Formulare)
- Brute-Force-Schutz (Login)
- Rate Limiting (API, Login)
- Sichere Passwort-Hashing
- Session-Management (Timeout, Fixation)
- Cookie-Flags (Secure, HttpOnly, SameSite)
- File-Upload Validierung
- Admin-Bereich abgesichert
- PCI-DSS Compliance (Zahlungsdaten)
- Keine sensiblen Daten in URLs
- Error-Pages ohne sensible Infos (kein Stack Trace)
- Dependency-Scanning (bekannte Vulnerabilities)
- Robots.txt verraet keine Admin-URLs

## 23. DSGVO (GDPR) / Datenschutz / Legal

- Cookie-Consent-Banner
- Cookie-Einstellungen aenderbar
- Nur notwendige Cookies ohne Consent
- Analytics erst nach Consent
- Marketing-Cookies erst nach Consent
- Datenschutzerklaerung (privacy policy) vollstaendig + aktuell
- Impressum (legal notice) vollstaendig
- AGB (Terms & Conditions) aktuell
- Widerrufsbelehrung (cancellation policy) + Muster-Formular
- Recht auf Datenloesung (right to erasure) — Account loeschen
- Datenexport (data portability) — DSGVO (GDPR) Art. 20
- Newsletter Double-Opt-in
- Checkbox-Texte korrekt formuliert
- Keine vorausgefuellten Checkboxen (Opt-in)
- Auftragsverarbeitungsvertraege (data processing agreements) — Third-Party
- Cookie-Laufzeiten dokumentiert

## 24. Analytics / Tracking

- Google Analytics 4 eingebunden
- GA4 Events korrekt (page_view, view_item, add_to_cart, purchase)
- Enhanced E-Commerce Tracking
- Conversion-Tracking (Google Ads, Meta Pixel)
- Google Tag Manager korrekt
- Server-Side Tracking (falls vorhanden)
- Consent-Mode v2 (Google)
- Facebook/Meta Pixel
- Pinterest Tag (falls relevant)
- Heatmap-Tool (Hotjar, Clarity)
- A/B-Testing Tool
- Tracking nach Consent-Ablehnung deaktiviert
- Revenue-Tracking korrekt (Waehrung, Betraege)
- Funnel-Tracking (Checkout-Schritte)

## 25. API / Schnittstellen

- Payment-Gateway APIs (Stripe, PayPal)
- Versanddienstleister API (DHL, DPD, GLS, Hermes)
- ERP-Anbindung
- Warenwirtschaft-Sync (inventory management sync)
- Buchhaltungs-Export (accounting export)
- Produktfeed (Google Shopping, Meta)
- Lagerverwaltung-Sync (warehouse management sync)
- CRM-Anbindung
- E-Mail-Service-Provider (Newsletter)
- Bewertungsplattform-API (Trustpilot, Ekomi)
- Preisvergleichsportale-Feed (price comparison portal feed)
- Marktplatz-Anbindungen (Amazon, eBay)
- Webhook-Verarbeitung
- API Rate-Limits
- API Fehlerbehandlung
- API Retry-Logik

## 26. Admin-Backend

- Admin Login / Authentifizierung
- Dashboard Uebersicht
- Bestellverwaltung
- Bestellstatus aendern
- Rechnung erstellen (create invoice)
- Lieferschein erstellen (create delivery note)
- Retouren verwalten (manage returns)
- Erstattungen durchfuehren (process refunds)
- Produktverwaltung (CRUD)
- Kategorieverwaltung
- Kundenverwaltung
- Gutscheinverwaltung (coupon management)
- CMS-Seitenverwaltung
- Medienverwaltung (Bilder, Dateien)
- Einstellungen (Shop, Versand, Zahlung, Steuern)
- Benutzer-/Rollenverwaltung
- Export-Funktionen (CSV, PDF)
- Import-Funktionen
- Statistiken / Auswertungen
- Log-Einsicht

## 27. Internationalisierung (falls relevant)

- Mehrsprachigkeit (Texte, URLs)
- Waehrungsumrechnung (currency conversion)
- Laenderspezifische Preise
- Laenderspezifische Versandkosten
- Laenderspezifische Zahlungsarten
- Laenderspezifische Steuersaetze (tax rates)
- Datumsformat laenderspezifisch
- Adressformat laenderspezifisch (PLZ-Feld etc.)

## 28. Infrastruktur / DevOps

- SSL-Zertifikat gueltig
- DNS korrekt konfiguriert
- Server-Monitoring
- Error-Logging (Sentry, Bugsnag)
- Backup-Strategie
- Deployment-Pipeline
- Staging-Umgebung vorhanden
- Rollback-Moeglichkeit
- Auto-Scaling (bei Traffic-Spitzen)
- Cron-Jobs / Scheduled Tasks
- Queue-System (E-Mails, Jobs)
- Redis/Cache-Layer

## 29. Edge Cases / Spezialszenarien

- Produkt waehrend Checkout nicht mehr verfuegbar
- Preis aendert sich waehrend Checkout
- Session laeuft waehrend Checkout ab
- Browser zurueck-Button im Checkout
- Doppelklick auf "Bestellen"
- Sehr langer Produktname
- Sehr viele Varianten (100+ Optionen)
- Leerer Warenkorb → direkt zum Checkout
- Warenkorb mit 50+ Artikeln
- Bestellung mit 0 EUR (Gutschein deckt alles)
- Gutschein hoeher als Bestellwert
- Sonderzeichen in Namen/Adressen (Umlaute, Akzente)
- Sehr lange Adressen
- Gleichzeitige Bestellung gleicher Artikel (Race Condition)
- Langsame Internetverbindung
- Checkout-Abbruch + Wiederaufnahme
- Mehrere Tabs gleichzeitig offen
- Inkognito-Modus
- Cookies deaktiviert
- JavaScript deaktiviert (Graceful Degradation)
- Adblocker aktiv

## 30. Drittanbieter-Integrationen

- Google Maps (Filialfinder, Kontaktseite)
- reCAPTCHA / Spam-Schutz
- Trusted Shops / Kaeufersiegel
- Bewertungswidgets (Trustpilot, Google Reviews)
- Social-Media Embeds
- YouTube/Vimeo Videos
- Chat-Widget (Zendesk, Intercom, Tidio)
- Push-Notifications
- Affiliate-Tracking
- Retargeting-Pixel

## 31. Retouren / Rueckgaben / Stornierungen

- Stornierung waehrend Bestellabwicklung (vor Versand)
- Stornierung nach Versand / waehrend Lieferung
- Rueckgabe-Antrag durch Kunden (Self-Service-Portal)
- RMA-Prozess (Return Merchandise Authorization)
- Ruecksendelabel-Generierung und -Download (return label generation)
- Erstattung auf Original-Zahlungsmethode (refund to original payment method)
- Teilerstattung bei Teilretoure (partial refund)
- Gutschrift statt Rueckerstattung (store credit instead of refund)
- Umtausch-Workflow (anderes Produkt/Variante)
- Rueckgabefrist-Pruefung (return period check — automatic rejection after deadline)
- Status-Updates fuer Retoure per E-Mail/SMS

## 32. Bestellstatus-Lifecycle / Order Management

- Vollstaendiger Status-Workflow: Eingegangen > Bezahlt > In Bearbeitung > Versendet > Zugestellt
- Teilversand (mehrere Pakete fuer eine Bestellung)
- Backorder / Nachbestellung bei Lieferengpass
- Bestellstatus-Seite fuer Kunden (ohne Login, per Link)
- Adressaenderung bei ausstehender Bestellung
- Verhinderung doppelter Bestellungen (Double-Submit-Protection)
- Zusammenfuehrung mehrerer Bestellungen
- Nachbestellung / Re-Order aus Bestellhistorie

## 33. Versand & Lieferung (vertieft)

- Versandzonen-Konfiguration (Inland, EU, International)
- Versandkosten-Berechnung nach Gewicht/Groesse/Ziel
- Kostenloser Versand ab Schwellenwert (free shipping threshold — Anzeige + korrekte Berechnung)
- Sendungsverfolgung / Tracking (shipment tracking)-Integration (DHL, DPD, GLS etc.)
- Lieferzeitschaetzung auf Produktseite und im Checkout
- Sonderziele: Packstation (parcel locker), Postfach (PO box), Inseln (islands), laendliche Adressen
- Expressversand-Aufpreis korrekt berechnet
- Click & Collect / Abholung im Geschaeft
- Lieferbenachrichtigungen (E-Mail/SMS bei Versand, Zustellung)

## 34. Steuer- & Preisberechnung

- MwSt-Berechnung (VAT calculation) fuer verschiedene Laender (DE 19%, AT 20%, CH 0% etc.)
- Netto/Brutto-Umschaltung (net/gross toggle — B2B vs. B2C)
- Rundungslogik bei Rabatten + MwSt-Kombination (rounding logic)
- Preiskonsistenz: Produktseite = Warenkorb = Checkout = Bestaetigung
- Waehrungsformatierung (currency formatting — Komma vs. Punkt, Symbol-Position)
- Staffelpreise / Mengenrabatte (tiered pricing / volume discounts) korrekt berechnet
- Preisanzeige bei konfigurierbaren Produkten (dynamische Aktualisierung)
- Durchgestrichene Preise / UVP (RRP) / Streichpreise (strikethrough prices) korrekt dargestellt
- Steuerausweisung auf Rechnung (tax disclosure on invoice — mandatory information)

## 35. Rechnungen & Dokumente

- PDF-Rechnung (invoice) generieren und herunterladen
- Lieferschein-Generierung (delivery note generation)
- Korrekte Pflichtangaben auf Rechnung (Steuernr (tax number), USt-IdNr (VAT ID), Adresse)
- Rechnung (invoice) im Kundenkonto abrufbar
- Rechnung per E-Mail versenden
- Gutschrift-Dokument bei Retoure (credit note for returns)
- Rechnungsnummern fortlaufend und korrekt (sequential invoice numbers)

## 36. Bestandsmanagement / Lager

- Echtzeit-Lagerbestand-Synchronisation
- "Ausverkauft"-Anzeige wenn Bestand = 0
- "Nur noch X auf Lager"-Hinweis bei niedrigem Bestand
- Verhinderung von Ueberverkauf (overselling prevention — Concurrency: 2 User kaufen letztes Stueck gleichzeitig)
- Bestandsreservierung waehrend Checkout (stock reservation — temporaere Sperre)
- Automatische Bestandsreduzierung nach Kauf
- Bestandserhoehung nach Stornierung/Retoure
- "Benachrichtigung bei Verfuegbarkeit"-Feature
- Variantenspezifischer Bestand (Farbe/Groesse hat eigenen Bestand)

## 37. Session-Management & Warenkorbpersistenz

- Warenkorb bleibt nach Browser-Schliessen erhalten (eingeloggt)
- Warenkorb-Merging: Gast-Warenkorb + Login-Warenkorb zusammenfuehren
- Session-Timeout-Verhalten (Checkout-Formular-Verlust?)
- Gleichzeitige Sessions auf mehreren Geraeten
- Cart-Abandonment-Trigger (Zeitpunkt der "Verlassen"-Erkennung)
- Warenkorbinhalte nach Passwort-Reset
- Cookie-Ablauf und Session-Wiederherstellung

## 38. Fehlerbehandlung & Graceful Degradation

- Verhalten bei Payment-Gateway-Ausfall (Fallback oder Fehlermeldung)
- Verhalten bei Datenbank-Verbindungsabbruch
- Verhalten bei externem API-Timeout (Versand, Steuer, etc.)
- Benutzerfreundliche Fehlermeldungen (kein Stack-Trace, keine technischen Details)
- 404-Seite mit Suchfunktion und Navigationshilfe
- 500-Fehler: Automatische Benachrichtigung an Admin
- Formular-Recovery nach Fehler (Eingaben nicht verloren)
- Circuit-Breaker-Pattern fuer externe Services
- Offline-Toleranz / Service-Worker-Caching

## 39. Betrugsschutz / Fraud Prevention

- Bot-Erkennung bei Login (Brute-Force-Schutz)
- Rate-Limiting bei Formular-Submissions
- CAPTCHA/reCAPTCHA an kritischen Stellen (Login, Registrierung, Checkout)
- Honeypot-Felder in Formularen
- Kreditkarten-Testing-Erkennung (viele fehlgeschlagene Zahlungen)
- IP-basierte Auffaelligkeiten erkennen
- Adress-Verification-System (AVS) bei Kreditkarten
- Velocity Checks (ungewoehnlich viele Bestellungen pro IP/Konto)

## 40. Benachrichtigungssystem (ueber E-Mail hinaus)

- Push-Notifications (Browser/Web-Push)
- SMS-Benachrichtigungen (Versand, Lieferung)
- In-App-Notifications im Kundenkonto
- Webhook-Zuverlaessigkeit (Retry bei Fehlschlag)
- Benachrichtigungs-Praeferenzen (Opt-in/Opt-out je Kanal)
- Benachrichtigung bei Preisaenderung (Wunschliste)
- Back-in-Stock-Benachrichtigung
- Bestellstatus-Updates per WhatsApp/Telegram

## 41. Personalisierung & Empfehlungen

- "Kuerzlich angesehen"-Widget korrekt befuellt
- "Aehnliche Produkte"-Empfehlungen relevant
- "Kunden kauften auch"-Logik
- "Haeufig zusammen gekauft"-Bundles
- Personalisierte Startseite basierend auf Browsing-History
- Empfehlungs-Widgets auf 404-Seite und leerer Suchergebnis-Seite
- A/B-Test-Faehigkeit fuer Empfehlungsalgorithmen

## 42. Social-Commerce & Sharing

- Social-Media-Share-Buttons auf Produktseiten
- Open-Graph-Tags (og:title, og:image, og:description) korrekt befuellt
- Twitter-Card-Tags korrekt
- Social Login (Facebook, Google, Apple)
- Share-Vorschau in sozialen Netzwerken (Bild + Text korrekt?)
- Social Proof: Anzeige "X Personen sehen sich dieses Produkt gerade an"
- Instagram Shopping / Facebook Shop Integration

## 43. Treueprogramm / Loyalty & Abonnements

- Punkte-Sammeln bei Kauf korrekt berechnet
- Punkte-Einloesen im Checkout
- Punktestand im Kundenkonto sichtbar
- Ablauf/Verfall von Punkten
- Abonnement/Subscription-Bestellungen (wiederkehrende Lieferung)
- Abo-Verwaltung (Intervall aendern, pausieren, kuendigen)
- Wiederkehrende Zahlung bei Abonnement
- Referral/Empfehlungsprogramm (Tracking des Einladungslinks)

## 44. Bulk-Operationen & Import/Export

- CSV/Excel-Produktimport (Preise, Beschreibungen, Bilder)
- Produktexport fuer Marktplaetze (Google Shopping, idealo)
- Massenbearbeitung von Produkten (Preisaenderung, Status)
- Datenvalidierung beim Import (fehlerhafte Zeilen melden)
- Import von Kundendaten / Bestelldaten (Migration)
- Export von Bestellungen fuer Buchhaltung/ERP

## 45. Visuelles Regressionstest

- Screenshot-Vergleich vor/nach Code-Aenderungen
- Konsistenz ueber verschiedene Seitentypen (PDP, PLP, Cart, Checkout)
- Font-Rendering und Typografie
- Farbkonsistenz und Brand-Compliance
- Icon-/Button-Konsistenz
- Responsive Breakpoints visuell pruefen
- Dark-Mode-Kompatibilitaet (falls vorhanden)

## 46. Navigation & UX-Patterns (vertieft)

- Breadcrumb-Navigation korrekt und vollstaendig
- Zurueck-Button-Verhalten (Browser-History korrekt?)
- Paginierung vs. "Mehr laden" vs. Infinite Scroll
- Scroll-Position-Wiederherstellung nach Zurueck-Navigation
- Sticky-Header/Navigation-Verhalten beim Scrollen
- Footer-Links (alle funktional, keine Deadlinks)
- Mega-Menu-Verhalten auf Desktop und Mobile
- Keyboard-Navigation (Tab-Reihenfolge logisch)
- Skip-to-Content-Link fuer Screenreader

## 47. Bilder & Medien

- Broken-Image-Erkennung (fehlende Produktbilder)
- Alt-Text fuer alle Produktbilder vorhanden und beschreibend
- Lazy-Loading korrekt (Above-the-fold NICHT lazy-loaden)
- Bildformat-Optimierung (WebP/AVIF mit Fallback)
- Produktgalerie: Zoom, Swipe, Fullscreen
- Video-Einbettung auf Produktseiten
- 360-Grad-Produktansicht (falls vorhanden)
- Bildgroessen-Konsistenz in Produktlisten
- Platzhalter/Skeleton waehrend Bildladung

## 48. Live-Chat & Kundensupport-Integration

- Chat-Widget laed und oeffnet korrekt
- Chat-Widget verdeckt keine kritischen UI-Elemente (CTA-Buttons!)
- Chatbot-Antworten auf Standardfragen korrekt
- Uebergabe Chatbot > menschlicher Agent funktioniert
- Chat-Verlauf bleibt bei Seitenwechsel erhalten
- Chat-Widget auf Mobile benutzbar
- Offline-Modus des Chats (Kontaktformular statt Live-Chat)
- Help-Center / FAQ-Seiten durchsuchbar

## 49. Disaster Recovery & Rollback

- Backup-Wiederherstellung getestet (DB + Dateien)
- Backup-Integritaet pruefen (keine leeren/korrupten Backups)
- Rollback-Plan bei fehlgeschlagenem Deployment
- RPO/RTO definiert und getestet
- Failover bei Server-Ausfall
- Datenbank-Replikation funktioniert
- Automatische Alerts bei System-Ausfall

## 50. Datenmigration & Validierung (Relaunch-spezifisch)

- Produktdaten vollstaendig migriert (Titel, Beschreibung, Preise, Bilder)
- Kundendaten korrekt (Adressen, Bestellhistorie, Passwort-Hashes)
- Bestellhistorie einsehbar und korrekt
- 301-Redirects fuer alle alten URLs eingerichtet
- SEO-Metadaten migriert (Title, Description, Canonical)
- Kategoriestruktur korrekt abgebildet
- Kundenbewertungen/Reviews migriert
- Gutschein-/Rabattcodes noch gueltig
- Warenkorbinhalte bestehender Kunden (wenn moeglich)
- Datensatz-Zaehlung: Quelle vs. Ziel vergleichen

## 51. Rechtliche Compliance (vertieft)

- Cookie-Consent-Banner erscheint beim ersten Besuch
- Cookies werden ERST nach Einwilligung gesetzt (kein Pre-Tracking)
- Cookie-Praeferenzen aenderbar und gespeichert
- Impressum (legal notice) vollstaendig und erreichbar
- AGB (Terms & Conditions) verlinkt und akzeptierbar im Checkout
- Widerrufsbelehrung (cancellation policy) korrekt dargestellt
- Datenschutzerklaerung (privacy policy) verlinkt und vollstaendig
- Altersverifikation (age verification — falls altersrelevante Produkte)
- Preisangabenverordnung / PAngV (Price Indication Regulation): Grundpreis (unit price), Gesamtpreis (total price)
- Widerrufsformular (cancellation form) als PDF downloadbar
- Buttonloesung (button solution — German legal req.): "Zahlungspflichtig bestellen" ("order with obligation to pay") auf Bestell-Button

## 52. Last- & Stresstests (vertieft)

- Spike-Tests: Ploetzlicher Traffic-Anstieg (z.B. Newsletter-Versand)
- Soak-Tests: Langzeit-Belastung ueber Stunden
- Checkout unter Last (parallele Bestellungen)
- Datenbank-Performance bei grossen Katalogen (>10.000 Produkte)
- CDN-Failover bei CDN-Ausfall
- Auto-Scaling getestet (wenn Cloud-Hosting)
- Wiederherstellungszeit nach Crash messen

## 53. B2B-spezifische Features (falls zutreffend)

- Kundengruppen mit unterschiedlichen Preisen
- Staffelpreise / Mengenrabatte (tiered pricing / volume discounts)
- Angebotsanfrage (request for quote / RFQ)
- Bestellung auf Rechnung (purchase on invoice — Zahlungsziel (payment terms) 30/60/90 Tage)
- Freigabe-Workflows (approval workflows — Bestellgenehmigung durch Vorgesetzten)
- Kundennummern-/Referenznummern bei Bestellungen (customer/reference numbers)
- Netto-Preisanzeige (net price display)
- Kundenspezifische Sortimente/Kataloge (customer-specific catalogs)
- Schnellbestellung per Artikelnummer (quick order by SKU)

---

## Quellen (Online-Recherche)

- [Katalon: 100 Test Cases for E-Commerce Website](https://katalon.com/resources-center/blog/test-cases-for-ecommerce-website)
- [BrowserStack: 60 Test Cases for E-Commerce Website](https://www.browserstack.com/guide/test-cases-for-ecommerce-website)
- [6amtech: 130+ Must-Have Test Cases for eCommerce](https://6amtech.com/blog/ecommerce-website-testing-and-test-cases-checklist/)
- [Shopify: E-Commerce Testing Guide 2026](https://www.shopify.com/blog/ecommerce-testing)
- [Maruti Techlabs: Guide to E-Commerce Website Testing](https://marutitech.com/guide-to-ecommerce-website-testing/)
- [TestGrid: 70 Test Cases for E-Commerce Websites](https://testgrid.io/blog/test-cases-for-ecommerce-website/)
- [TestGrid: Black Friday E-Commerce Testing Tips](https://testgrid.io/blog/black-friday-ecommerce-testing/)
- [bugbug.io: E-Commerce Testing Guide 2026](https://bugbug.io/blog/software-testing/ecommerce-testing/)
- [bugbug.io: Test Cases Best Practices and Templates](https://bugbug.io/blog/software-testing/test-cases-for-ecommerce-website/)
- [Lyssna: E-Commerce Website Testing Checklist](https://www.lyssna.com/guides/website-testing/ecommerce-website-testing-checklist/)
- [InstaWP: E-Commerce Migration Checklist (Go-Live + Post-Launch)](https://instawp.com/ecommerce-migration-checklist/)
- [Cloudways: Complete E-Commerce Migration Checklist 2026](https://www.cloudways.com/blog/ecommerce-migration-checklist/)
- [Eat Fresh Tech: 12-Point E-Commerce Disaster Recovery Plan](https://www.eatfresh.tech/blog/12-point-ecommerce-disaster-recovery-plan-checklist)
- [OWASP: Session Timeout Testing Guide](https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/07-Testing_Session_Timeout)
