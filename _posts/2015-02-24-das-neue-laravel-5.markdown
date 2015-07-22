---
layout: post
title:  "Das neue Laravel 5 – PHP Framework für Web-Künstler"
date:   2015-02-24 19:32:03
categories: laravel
---

[![Laravel 5 - Love beautiful code? We do too!](http://www.onlinewerk.info/wp-content/uploads/2015/02/Bildschirmfoto-2015-02-07-um-18.16.14-611x313.png)](http://www.onlinewerk.info/wp-content/uploads/2015/02/Bildschirmfoto-2015-02-07-um-18.16.14.png)


Laravel hat sich seit dem ersten Release 2011 zum mittlerweile [beliebtesten PHP Framework](//www.sitepoint.com/best-php-frameworks-2014/) der Welt entwickelt. Von vielen wird Laravel die Rettung von PHP genannt. Mit einer unglaublich guten Dokumentation, einer einfachen und übersichtlichen Code-Struktur und jeder Menge Features überrannte Laravel die PHP-Community im Sturm. Nach mehreren verschobenen Release-Terminen wurde am 4\. Februar 2015 die aktuelle Version 5 von Laravel veröffentlicht.

> Some years ago I was ashamed to say I was a PHP developer, today I can't be more proud. Thank you [@laravelphp](https://twitter.com/laravelphp) [pic.twitter.com/1TioslbdCz](http://t.co/1TioslbdCz) — Duilio Palacios (@Sileence) [February 4, 2015](https://twitter.com/Sileence/status/562998276879044611)

Version 5 bringt einige Änderungen von denen einige in diesem Artikel kurz vorgestellt werden. Laravel bietet natürlich noch viele weitere tolle Features welche alle sehr ausführlich in der offiziellen [Dokumentation](http://laravel.com/docs/5.0 "Laravel 5 Dokumentation") beschrieben werden.

## **Dateistruktur**

In früheren Versionen von Laravel war praktisch die gesamte Applikation im Ordner app/ zu finden. Mit Laravel 5 wird dieser Ordner nun kräftig entstaubt und beinhaltet nur noch die Logik der Applikation selbst. Konfigurationsdateien und datenbank-relevante Daten wurden ins Hauptverzeichnis darüber verschoben. Weil viele Entwickler bereits in Laravel 4.2 den Models Ordner entfernten um die Models direkt in den entsprechenden Bereich der Business-Logik zu packen, wurde der Models Ordner in Laravel 5 nun komplett entfernt. Es bleibt jedoch dem Entwickler überlassen, ob er diesen doch wieder erstellen möchte. Unter resources/ finden sich nun Views, Sprachdateien und andere Assets. Insgesamt ist die Dateistruktur von Laravel 5 wesentlich aufgeräumter und bietet sinnvolle Strukturierung auch für große Projekte.

## **Blade Output**

In Laravel gab es für die Templatesprache Blade doppelte und dreifache geschwungene Klammern für einen Raw-Output und einen gesäuberten Output (Escaped). Beispiel:

<pre>$name = "<script>BöserSchadcode</script>"

{{$name}} //output: <script>BöserSchadcode</script>

{{{$name}}} //output: BöserSchadcode
</pre>

Ab Laravel 5 liefern beide nun den gesäuberten Output. Will man die Rohdaten ausgeben, muss man jetzt Ausrufezeichen verwenden:

<pre>{!! $name !!}</pre>

Das hat den Vorteil dass nun eine extra Syntax für Rohdaten und die Standardsyntax für gesäuberte Daten gilt. Somit ist es schwieriger aus Versehen gewisse Ausgaben zu säubern und damit die Anwendung verwundbar gegen Angriffe zu machen.

## **Namespacing**

Endlich finden sich in Laravel 5 auch Namespaces. Standardmäßig wird hier der Namespace „App“ verwendet und verweist per PSR-4 autoloading direkt auf den Ordner app/. Der Namespace kann aber einfach umbenannt werden und Laravel ersetzt automatisch alle Instanzen des „App/“-Namespaces in bestehenden Laravel-Klassen.

<pre>php artisan app:name Onlinewerk</pre>

Mit diesem Befehl werden alle Standardklassen im app/ Ordner mit „Onlinewerk“ _genamespaced_.

## **Form Requests**

Form Requests sind ein neues Feature in Laravel 5 mit dem Form-Validierung und Autorisierung einen eigenen Platz bekommen haben. Models und Controller werden nun also von der Validierungs-Logik befreit. Form Requests bieten sich bei komplexen Validierungen an, um die Logik auszulagern. Per Artisan Befehl kann ein neuer Form Request erstellt werden:

<pre>php artisan make:request StorePostRequest</pre>

Die Request-Klasse wird im app/Http/Requests/ Ordner erstellt und beinhaltet standardmäßig ein Rules-Array und die Methode authorize(). Die Rules dienen der Validierung, in der Methode können Rechte überprüft werden (z.B. ob der User eingeloggt oder über 18 Jahr alt ist). Der folgende Form Request zeigt die Validierung einer Benutzer-Registrierung:

<pre>class UserFormRequest extends FormRequest {

    public function rules()
    {
        return [
            'username' => 'required|alpha_dash',
            'email'   => 'required|email',
            'password' => 'required|between:8,50|confirmed',
        ];
    }

    public function authorize()
    {
        // Wir geben hier einfach true zurück, 
        // weil sich jeder registrieren können soll.

        return true;
    }

}</pre>

In einem Controller kann nun das Request Objekt verwendet werden, indem man es einfach in eine gewünschte Methode injiziert. Bevor der Inhalt der Methode aufgerufen wird, findet die Validierung im Form Request statt.

<pre>class UsersController extends Controller {

    public function postRegister(**UserFormRequest $request**)
    {
        // .... Code um User zu registrieren ...

        return Response::make('Registrierung erfolgreich!');
    }

    // weitere Methoden des Controllers

}
</pre>

## **Route Caching**

Die beliebten Routen in Laravel haben nun ihren eigenen Cache. Dieser hilft besonders bei großen Anwendungen mit vielen definierten Routes und kann dort einige hundert Millisekunden Ladezeit sparen. Das ganze kann per Artisan-Befehl aufgerufen werden:

<pre>php artisan route:cache</pre>

Dieser Befehl muss nach jeder weiteren Änderung der Routes erneut aufgerufen werden.

## **Method Injection**

Dass Laravel einfach Methoden injizieren kann wie im vorherigen Code-Beispiel gezeigt, ist auch ab Version 5 neu. Üblicherweise wurden Abhängikeiten im Konstruktor geladen, nun können diese auch direkt in einer Methode geladen werden.

<pre>public function speichern(**Request $request**) {}</pre>

## **Environments**

Environment Detection wurde in Laravel 5 komplett überarbeitet. Hier wird nun auf das PHP Package dotenv gesetzt. Im Root-Ordner können Dateien mit der .env Endung erstellt werden und beinhalten z.B. die Zugangsdaten der Datenbank für die lokale Entwicklung. In der Datenbank-Config werden dann über eine Funktion die richtigen Daten geladen.

<pre>return array(
    'database' => env('DB_DATABASE’, 'productiondb'),
    'user' => env('DB_USER, 'productiondb'),
);
</pre>

Die Funktion env() sucht dabei je nach derzeitigem Environment den Wert von „DB_DATABASE” und „DB_USER“ in der entsprechenden “.env” Datei. So können nun einzelne Dateien für die Live- und die Test-Umgebung angelegt werden.

## **Social Authentication**

Mit Laravel 5 kommt auch ein geniales, und optionales, Package was die Authentifizierung mit OAuth-Anbietern um ein vielfaches erleichtert. Das Package Socialite unterstützt zur Zeit Facebook, Twitter, Google und GitHub. Mit Socialite lassen sich mit wenigen Zeilen Code Registrierungs-Formulare mit Facebook- und Twitter-Registierung bauen. Zwei Routes verarbeiten dabei die Anfrage und die zurückkommenden Benutzerdaten.

<pre>public function redirectToProvider()
{
    return Socialize::with('github')->redirect();
}

public function handleProviderCallback()
{
    $user = Socialize::with('github')->user();

    // $user->token;
}
</pre>

Nach der Anfrage per Socialite kann ganz bequem auf das User-Objekt zugegriffen werden:

<pre>$user->getNickname();

$user->getName();

$user->getEmail();

$user->getAvatar();</pre>

## **Fazit**

Beim aktuellen major release von Laravel hat sich mal wieder einiges getan und in diesem Artikel wurde auch nur ein kleiner Teil der Neuerungen vorgestellt. Weitere tolle Features wie Elixir, Middlewares und Schedulers können der umfassenden Dokumentation entnommen werden. Auf Laracasts (Lernplattform für PHP und Laravel) gibt es mehrere kostenlose Video-Serien zu Laravel 5\. [Laravel 5 Fundamentals](https://laracasts.com/series/laravel-5-fundamentals) und [What's New in Laravel 5.0 (Alpha)](https://laracasts.com/series/whats-new-in-laravel-5/episodes/3) sind sehr zu empfehlen.