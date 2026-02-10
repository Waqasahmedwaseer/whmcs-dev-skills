---
name: whmcs-dev-skills
description: >
  Senior WHMCS Developer & Architect skill for AI coding agents. Builds,
  debugs, and maintains WHMCS Addon Modules, Provisioning (Server) Modules,
  Domain Registrar Modules, Payment Gateway Modules, and Action Hooks.
  Enforces WHMCS 8.x / 9.x best practices, modern PHP 8.1+ standards,
  Laravel Capsule ORM, Smarty v4 templating, and PSR-1/PSR-2 coding style.
  Use this skill whenever a user needs to create, modify, debug, or audit
  any WHMCS module, hook, or integration.
license: GPL-2.0
compatibility: >
  Works with all AI coding agents including Claude Code, GitHub Copilot,
  Cursor, Windsurf, VS Code, Amp, Goose, and OpenCode. Requires PHP 8.1+
  and WHMCS 8.x or 9.x environment for generated code.
metadata:
  author: waqasahmedwaseer
  version: "1.0.0"
  website: "https://waqasahmedwaseer.com"
---

# WHMCS Dev Skills — AI Agent Skill

> **Scope**: Full-stack WHMCS module development covering Addon Modules,
> Provisioning (Server) Modules, Domain Registrar Modules, Payment Gateway
> Modules (Third-Party, Merchant, Tokenised), Action Hooks, Internal/External
> API integration, and Theme/Template customisation.

---

## Table of Contents

1. [Operational Boundaries](#1-operational-boundaries)
2. [Platform Requirements](#2-platform-requirements)
3. [Coding Standards](#3-coding-standards)
4. [Database Operations](#4-database-operations)
5. [Module Development](#5-module-development)
   - 5.1 [Addon Modules](#51-addon-modules)
   - 5.2 [Provisioning (Server) Modules](#52-provisioning-server-modules)
   - 5.3 [Domain Registrar Modules](#53-domain-registrar-modules)
   - 5.4 [Payment Gateway Modules](#54-payment-gateway-modules)
6. [Action Hooks](#6-action-hooks)
7. [API Integration](#7-api-integration)
8. [Templating & UI](#8-templating--ui)
9. [Security Checklist](#9-security-checklist)
10. [Error Handling & Logging](#10-error-handling--logging)
11. [Module Upgrade Pattern](#11-module-upgrade-pattern)
12. [Common Pitfalls & Anti-Patterns](#12-common-pitfalls--anti-patterns)
13. [Project Structure Templates](#13-project-structure-templates)
14. [Quick-Reference Code Snippets](#14-quick-reference-code-snippets)
15. [Official References](#15-official-references)

---

## 1. Operational Boundaries

### ✅ ALWAYS

- Add `defined("WHMCS") or die("Access Denied");` as the **first line** of
  every PHP file.
- Use `Illuminate\Database\Capsule\Manager` (Laravel Capsule) for **all**
  database operations.
- Use `logModuleCall()` for **every** external API request to enable the
  WHMCS Module Log (Configuration → System Logs → Module Log).
- Use `logActivity()` to write meaningful entries to the System Activity Log.
- Use Smarty `.tpl` template files for **all** HTML output — never echo raw
  HTML in logic files.
- Follow PSR-1 and PSR-2 coding standards.
- Use `<?php` full opening tags; omit the closing `?>` tag in pure-PHP files.
- Wrap all external API calls and database schema changes in `try/catch`
  blocks.
- Use parameter binding (Capsule / PDO) — **never** concatenate user input
  into SQL.
- Validate and sanitise all `$_POST` and `$_GET` input.
- Prefix custom database tables with `mod_` (e.g., `mod_yourmodule_data`).
- Provide a `lang/english.php` language file for every module.
- Run unit/integration tests before committing module changes.
- Write code compatible with **PHP 8.1+** (prefer 8.2 / 8.3) with strict
  type hints.
- Test in a **staging environment** before deploying to production.

### ⚠️ ASK FIRST

- Before performing bulk refunds or mass invoice operations.
- Before performing `DROP TABLE` operations in deactivation functions.
- Before changing a client's password or authentication settings.
- Before modifying any server-level configuration.
- Before deleting or merging client accounts.
- Before performing any action that cannot be undone.

### 🚫 NEVER

- Modify WHMCS core files (`/admin/`, `/includes/`, `/vendor/`). Use Hooks
  or Modules instead.
- Modify `configuration.php` directly.
- Use `mysql_*`, `mysqli_*`, or raw PDO — always use Capsule.
- Use deprecated `{php}` tags in Smarty templates.
- Use `$_REQUEST` — be explicit with `$_POST` or `$_GET`.
- Hardcode absolute file paths — use `ROOTDIR`, `$CONFIG['SystemURL']`, or
  WHMCS constants.
- Store sensitive data (passwords, API keys) in plain text — use WHMCS's
  `encrypt()` / `decrypt()` helpers.
- Output detailed error messages to end-users in production — log them
  internally instead.
- Use `echo` or `print` for output in module files — return structured arrays
  or use Smarty templates.

---

## 2. Platform Requirements

| Component          | WHMCS 8.x (8.11+)       | WHMCS 9.x               |
|--------------------|--------------------------|--------------------------|
| **PHP**            | 8.1 min, 8.2 recommended | 8.2 min, 8.3 recommended |
| **ionCube Loader** | 13.0.2+                  | 13.0.2+ (14.4.0 rec.)   |
| **Smarty**         | v3.1.x                   | v4.3.4                   |
| **GuzzleHTTP**     | v7.4                     | v7.4.5                   |
| **Illuminate**     | v7.x                     | v9.0                     |
| **MySQL/MariaDB**  | 5.7+ / 10.2+             | 8.0+ / 10.6+            |

### Key WHMCS 9.x Breaking Changes

- **Smarty v4**: Legacy Smarty tags are deprecated and will not function.
  Smarty Template Objects are no longer supported. All templates must use
  Smarty v4 syntax.
- **Illuminate v9**: Database relation methods and Eloquent patterns from
  Illuminate v7 may cause fatal errors. Test all Capsule queries.
- **PHP 8.2+**: Dynamic properties are deprecated. All class properties must
  be explicitly declared. Use constructor promotion where appropriate.

---

## 3. Coding Standards

Based on the [WHMCS Style Guide](https://developers.whmcs.com/modules/style-guide/):

```
✓  Use <?php ?> full tags only (never <? ?>).
✓  Omit closing ?> in pure-PHP files.
✓  Indent with 4 spaces (no tabs).
✓  No trailing whitespace on lines.
✓  Use Unix line endings (\n), not Windows (\r\n).
✓  End every file with a single newline.
✓  UTF-8 encoding without BOM.
✓  Follow PSR-1 (Basic Coding Standard) and PSR-2 (Coding Style Guide).
✓  Use strict_types declaration: declare(strict_types=1);
✓  Use PHP 8.1+ type hints for all parameters, return types, and properties.
```

### Naming Conventions

| Element             | Convention                     | Example                          |
|---------------------|--------------------------------|----------------------------------|
| Module Directory    | lowercase, letters & numbers   | `mymodule` or `my_module`        |
| Module Functions    | `{modulename}_FunctionName`    | `mymodule_config()`              |
| Hook Functions      | Unique prefixed name           | `mymodule_hookClientAdd()`       |
| Database Tables     | `mod_{modulename}_{entity}`    | `mod_mymodule_settings`          |
| Config Fields       | camelCase keys                 | `apiKey`, `secretToken`          |
| Template Files      | lowercase with hyphens         | `admin-dashboard.tpl`            |
| Language Keys       | snake_case                     | `module_description`             |

---

## 4. Database Operations

### ✅ Modern Pattern — Laravel Capsule

```php
<?php

use Illuminate\Database\Capsule\Manager as Capsule;

// SELECT
$clients = Capsule::table('tblclients')
    ->where('status', '=', 'Active')
    ->orderBy('lastname', 'asc')
    ->get();

// SELECT single row
$client = Capsule::table('tblclients')
    ->where('id', '=', $clientId)
    ->first();

// INSERT
Capsule::table('mod_mymodule_logs')->insert([
    'client_id' => $clientId,
    'action'    => 'provisioned',
    'created_at' => date('Y-m-d H:i:s'),
]);

// UPDATE
Capsule::table('tblhosting')
    ->where('id', '=', $serviceId)
    ->update(['notes' => 'Updated via module']);

// DELETE (use with caution)
Capsule::table('mod_mymodule_logs')
    ->where('created_at', '<', $cutoffDate)
    ->delete();
```

### Schema Creation (in `_activate`)

```php
<?php

use Illuminate\Database\Capsule\Manager as Capsule;

try {
    Capsule::schema()->create('mod_mymodule_data', function ($table) {
        /** @var \Illuminate\Database\Schema\Blueprint $table */
        $table->increments('id');
        $table->unsignedInteger('client_id');
        $table->string('key', 255);
        $table->text('value')->nullable();
        $table->timestamps();

        $table->index('client_id');
        $table->unique(['client_id', 'key']);
    });
} catch (\Exception $e) {
    return [
        'status'      => 'error',
        'description' => 'Unable to create table: ' . $e->getMessage(),
    ];
}
```

### Schema Updates (in `_upgrade`)

```php
<?php

use Illuminate\Database\Capsule\Manager as Capsule;

function mymodule_upgrade($vars)
{
    $currentVersion = $vars['version'];

    if (version_compare($currentVersion, '1.1.0', '<')) {
        Capsule::schema()->table('mod_mymodule_data', function ($table) {
            $table->string('status', 50)->default('active')->after('value');
        });
    }

    if (version_compare($currentVersion, '1.2.0', '<')) {
        Capsule::schema()->table('mod_mymodule_data', function ($table) {
            $table->unsignedInteger('order_id')->nullable()->after('client_id');
            $table->index('order_id');
        });
    }
}
```

### 🚫 Forbidden Pattern — Raw SQL

```php
// ❌ NEVER DO THIS
$result = mysql_query("SELECT * FROM tblclients WHERE id = $clientId");
$result = mysqli_query($conn, "SELECT * FROM tblclients WHERE id = '$clientId'");
$result = full_query("SELECT * FROM tblclients WHERE id = " . $_GET['id']);
```

---

## 5. Module Development

### Module Naming Rules (All Types)

- Names must be **all lowercase**.
- Names must **start with a letter**.
- Only **letters, numbers, and underscores** are allowed.
- Name must be **unique** across the WHMCS installation.
- The directory name and main PHP filename must match exactly.

---

### 5.1 Addon Modules

**Path**: `/modules/addons/{modulename}/`

**Official Sample**: [github.com/WHMCS/sample-addon-module](https://github.com/WHMCS/sample-addon-module)

#### Required Functions

| Function                     | Purpose                                  |
|------------------------------|------------------------------------------|
| `{name}_config()`            | Module metadata + configuration fields   |
| `{name}_activate()`          | Runs on module activation (create tables)|
| `{name}_deactivate()`        | Runs on module deactivation (cleanup)    |
| `{name}_output($vars)`       | Admin area page output (echo, not return)|
| `{name}_clientarea($vars)`   | Client area output (return array)        |
| `{name}_upgrade($vars)`      | Handles version upgrades (schema changes)|

#### Config Function Template

```php
<?php

defined("WHMCS") or die("Access Denied");

use Illuminate\Database\Capsule\Manager as Capsule;
use WHMCS\Module\Addon\Setting;

function mymodule_config(): array
{
    return [
        'name'        => 'My Module',
        'description' => 'A brief description of what this module does.',
        'version'     => '1.0.0',
        'author'      => 'Your Name',
        'language'    => 'english',
        'fields'      => [
            'apiUrl' => [
                'FriendlyName' => 'API URL',
                'Type'         => 'text',
                'Size'         => '60',
                'Default'      => 'https://api.example.com',
                'Description'  => 'The base URL of the external API.',
            ],
            'apiKey' => [
                'FriendlyName' => 'API Key',
                'Type'         => 'password',
                'Size'         => '40',
                'Description'  => 'Your API authentication key.',
            ],
            'enableDebug' => [
                'FriendlyName' => 'Debug Mode',
                'Type'         => 'yesno',
                'Description'  => 'Enable verbose logging for troubleshooting.',
            ],
            'syncInterval' => [
                'FriendlyName' => 'Sync Interval',
                'Type'         => 'dropdown',
                'Options'      => '1 Hour,6 Hours,12 Hours,24 Hours',
                'Default'      => '6 Hours',
                'Description'  => 'How often to sync data.',
            ],
        ],
    ];
}
```

#### Activate / Deactivate Template

```php
<?php

function mymodule_activate(): array
{
    try {
        Capsule::schema()->create('mod_mymodule_data', function ($table) {
            /** @var \Illuminate\Database\Schema\Blueprint $table */
            $table->increments('id');
            $table->unsignedInteger('client_id');
            $table->string('key', 255);
            $table->text('value')->nullable();
            $table->timestamps();
            $table->index('client_id');
        });

        return [
            'status'      => 'success',
            'description' => 'Module activated successfully. Configure settings above.',
        ];
    } catch (\Exception $e) {
        return [
            'status'      => 'error',
            'description' => 'Failed to create database table: ' . $e->getMessage(),
        ];
    }
}

function mymodule_deactivate(): array
{
    try {
        Capsule::schema()->dropIfExists('mod_mymodule_data');

        return [
            'status'      => 'success',
            'description' => 'Module deactivated and data cleaned up.',
        ];
    } catch (\Exception $e) {
        return [
            'status'      => 'error',
            'description' => 'Failed to remove database table: ' . $e->getMessage(),
        ];
    }
}
```

#### Admin Output Function

```php
<?php

function mymodule_output($vars): void
{
    $moduleLink = $vars['modulelink'];
    $version    = $vars['version'];
    $apiUrl     = $vars['apiUrl'];
    $apiKey     = $vars['apiKey'];
    $LANG       = $vars['_lang'];

    $action = $_GET['action'] ?? 'index';

    switch ($action) {
        case 'settings':
            // Settings page
            echo '<h2>' . ($LANG['settings_title'] ?? 'Settings') . '</h2>';
            break;

        default:
            // Dashboard / index page
            echo '<h2>' . ($LANG['dashboard_title'] ?? 'Dashboard') . '</h2>';
            echo '<p>Module Version: ' . htmlspecialchars($version) . '</p>';
            echo '<a href="' . $moduleLink . '&action=settings" '
               . 'class="btn btn-default">Settings</a>';
            break;
    }
}
```

#### Client Area Function

```php
<?php

function mymodule_clientarea($vars): array
{
    $moduleLink = $vars['modulelink'];
    $LANG       = $vars['_lang'];

    return [
        'pagetitle'    => $LANG['client_page_title'] ?? 'My Module',
        'breadcrumb'   => [
            'index.php?m=mymodule' => 'My Module',
        ],
        'templatefile' => 'views/client/dashboard',
        'requirelogin' => true,
        'forcessl'     => true,
        'vars'         => [
            'moduleLink' => $moduleLink,
            'data'       => Capsule::table('mod_mymodule_data')
                ->where('client_id', '=', $_SESSION['uid'] ?? 0)
                ->get()
                ->toArray(),
        ],
    ];
}
```

---

### 5.2 Provisioning (Server) Modules

**Path**: `/modules/servers/{modulename}/`

**Official Sample**: [github.com/WHMCS/sample-provisioning-module](https://github.com/WHMCS/sample-provisioning-module)

#### Required & Optional Functions

| Function                          | Required | Purpose                                  |
|-----------------------------------|----------|------------------------------------------|
| `{name}_MetaData()`              | Yes      | Module metadata and capabilities         |
| `{name}_ConfigOptions()`         | Yes      | Product configuration fields             |
| `{name}_CreateAccount($params)`  | Yes      | Provision a new service                  |
| `{name}_SuspendAccount($params)` | Yes      | Suspend an overdue service               |
| `{name}_UnsuspendAccount($params)`| Yes     | Unsuspend after payment                  |
| `{name}_TerminateAccount($params)`| Yes     | Terminate a service                      |
| `{name}_Renew($params)`          | Optional | Handle renewal payments                  |
| `{name}_ChangePassword($params)` | Optional | Client-initiated password change         |
| `{name}_ChangePackage($params)`  | Optional | Handle upgrades/downgrades               |
| `{name}_TestConnection($params)` | Optional | Test server connectivity from admin      |
| `{name}_ClientArea($params)`     | Optional | Client area output for the service       |
| `{name}_AdminLink($params)`      | Optional | Quick-link on server config page         |
| `{name}_LoginLink($params)`      | Optional | Login link on product management page    |
| `{name}_UsageUpdate($params)`    | Optional | Daily disk/bandwidth import              |
| `{name}_AdminCustomButtonArray()`| Optional | Custom admin action buttons              |
| `{name}_ClientAreaCustomButtonArray()`| Optional | Custom client action buttons         |
| `{name}_ClientAreaAllowedFunctions()`| Optional | Hidden callable client functions      |
| `{name}_AdminServicesTabFields($params)`| Optional | Extra fields on admin services tab  |
| `{name}_AdminServicesTabFieldsSave($params)`| Optional | Save extra admin fields          |

#### Provisioning Return Pattern

```php
<?php

function myserver_CreateAccount(array $params): string
{
    try {
        $apiUrl   = $params['serverhostname'];
        $apiUser  = $params['serverusername'];
        $apiPass  = $params['serverpassword'];
        $domain   = $params['domain'];
        $username = $params['username'];
        $password = $params['password'];
        $package  = $params['configoption1'];

        // Make API call to create account on remote server
        $response = myserver_apiCall($apiUrl, 'createAccount', [
            'domain'   => $domain,
            'username' => $username,
            'password' => $password,
            'package'  => $package,
        ]);

        // Log the module call for debugging
        logModuleCall(
            'myserver',
            'CreateAccount',
            ['domain' => $domain, 'username' => $username, 'package' => $package],
            $response,
            null,
            [$apiPass] // Scrub password from logs
        );

        if ($response['success']) {
            return 'success';
        }

        return 'Error: ' . ($response['message'] ?? 'Unknown error');
    } catch (\Exception $e) {
        logModuleCall('myserver', 'CreateAccount', $params, $e->getMessage());
        return 'Error: ' . $e->getMessage();
    }
}
```

#### Key `$params` Variables (Provisioning)

```
$params['serverhostname']   // Server hostname
$params['serverusername']   // Server username
$params['serverpassword']   // Server password (use decrypt() if needed)
$params['serveraccesshash'] // Server access hash
$params['serversecure']     // SSL/TLS enabled (true/false)
$params['serverport']       // Server port
$params['domain']           // Client's domain
$params['username']         // Service username
$params['password']         // Service password
$params['clientsdetails']   // Array of client information
$params['configoption1']    // First config option value
$params['configoption2']    // Second config option value
$params['customfields']     // Custom field values
$params['serviceid']        // Service ID in tblhosting
$params['pid']              // Product ID
$params['model']            // Service model object (WHMCS 8+)
```

---

### 5.3 Domain Registrar Modules

**Path**: `/modules/registrars/{modulename}/`

**Official Sample**: [github.com/WHMCS/sample-registrar-module](https://github.com/WHMCS/sample-registrar-module)

#### Required & Optional Functions

| Function                           | Required | Purpose                              |
|------------------------------------|----------|--------------------------------------|
| `{name}_getConfigArray()`          | Yes      | Configuration fields                 |
| `{name}_RegisterDomain($params)`  | Yes      | Register a new domain                |
| `{name}_TransferDomain($params)`  | Yes      | Initiate domain transfer             |
| `{name}_RenewDomain($params)`     | Yes      | Renew an existing domain             |
| `{name}_GetNameservers($params)`  | Yes      | Retrieve current nameservers         |
| `{name}_SaveNameservers($params)` | Yes      | Update nameservers                   |
| `{name}_GetContactDetails($params)`| Optional| Get WHOIS contact info              |
| `{name}_SaveContactDetails($params)`| Optional| Update WHOIS contact info          |
| `{name}_GetEPPCode($params)`      | Optional | Retrieve EPP/auth code              |
| `{name}_GetRegistrarLock($params)` | Optional | Check registrar lock status         |
| `{name}_SaveRegistrarLock($params)`| Optional | Toggle registrar lock               |
| `{name}_GetDNS($params)`          | Optional | Get DNS host records                 |
| `{name}_SaveDNS($params)`         | Optional | Update DNS host records              |
| `{name}_GetEmailForwarding($params)`| Optional | Get email forwarding rules         |
| `{name}_SaveEmailForwarding($params)`| Optional | Update email forwarding           |
| `{name}_IDProtectToggle($params)` | Optional | Toggle ID Protection                 |
| `{name}_RegisterNameserver($params)`| Optional | Create private nameserver          |
| `{name}_ModifyNameserver($params)` | Optional | Update private nameserver           |
| `{name}_DeleteNameserver($params)` | Optional | Remove private nameserver           |
| `{name}_Sync($params)`            | Optional | Domain sync (status + expiry)        |
| `{name}_TransferSync($params)`    | Optional | Transfer status sync                 |
| `{name}_CheckAvailability($params)`| Optional | Domain availability check           |
| `{name}_GetDomainSuggestions($params)`| Optional | Domain name suggestions          |
| `{name}_RequestDelete($params)`   | Optional | Delete/cancel domain                 |

#### Registrar Return Pattern

```php
<?php

function myregistrar_RegisterDomain(array $params): array
{
    $sld = $params['sld'];
    $tld = $params['tld'];
    $domain = $sld . '.' . $tld;
    $regPeriod = $params['regperiod'];

    // Nameservers
    $ns1 = $params['ns1'];
    $ns2 = $params['ns2'];
    $ns3 = $params['ns3'] ?? '';
    $ns4 = $params['ns4'] ?? '';
    $ns5 = $params['ns5'] ?? '';

    // Contact details
    $firstName = $params['firstname'];
    $lastName  = $params['lastname'];
    $email     = $params['email'];
    // ... additional contact fields

    try {
        $response = myregistrar_apiCall('registerDomain', [
            'domain'    => $domain,
            'period'    => $regPeriod,
            'ns'        => array_filter([$ns1, $ns2, $ns3, $ns4, $ns5]),
            'contacts'  => [
                'firstName' => $firstName,
                'lastName'  => $lastName,
                'email'     => $email,
            ],
        ]);

        logModuleCall('myregistrar', 'RegisterDomain', [
            'domain' => $domain, 'period' => $regPeriod,
        ], $response);

        if ($response['success']) {
            return ['success' => true];
        }

        return ['error' => $response['message'] ?? 'Registration failed'];
    } catch (\Exception $e) {
        logModuleCall('myregistrar', 'RegisterDomain', $params, $e->getMessage());
        return ['error' => $e->getMessage()];
    }
}
```

#### Domain Sync Function

```php
<?php

function myregistrar_Sync(array $params): array
{
    $domain = $params['sld'] . '.' . $params['tld'];

    try {
        $response = myregistrar_apiCall('getDomainInfo', ['domain' => $domain]);

        logModuleCall('myregistrar', 'Sync', ['domain' => $domain], $response);

        return [
            'active'          => ($response['status'] === 'active'),
            'cancelled'       => ($response['status'] === 'cancelled'),
            'transferredAway' => ($response['status'] === 'transferred'),
            'expirydate'      => $response['expiryDate'], // Format: YYYY-MM-DD
        ];
    } catch (\Exception $e) {
        logModuleCall('myregistrar', 'Sync', $params, $e->getMessage());
        return ['error' => $e->getMessage()];
    }
}
```

---

### 5.4 Payment Gateway Modules

**Path**: `/modules/gateways/{modulename}.php`

**Callback Path**: `/modules/gateways/callback/{modulename}.php`

**Official Samples**:
- Third-Party: [github.com/WHMCS/sample-gateway-module](https://github.com/WHMCS/sample-gateway-module)
- Merchant: [github.com/WHMCS/sample-merchant-gateway](https://github.com/WHMCS/sample-merchant-gateway)

#### Gateway Types

| Type              | User Experience                              | Key Function     |
|-------------------|----------------------------------------------|------------------|
| **Third-Party**   | Customer leaves site to pay, returns after   | `_link()`        |
| **Merchant**      | Card details entered in WHMCS, processed BG  | `_capture()`     |
| **Tokenised**     | Card details not stored locally, token used  | `_storeremote()` |

#### Third-Party Gateway — Link Function

```php
<?php

function mygateway_link(array $params): string
{
    // Gateway configuration
    $merchantId = $params['merchantId'];
    $secretKey  = $params['secretKey'];

    // Invoice details
    $invoiceId   = $params['invoiceid'];
    $amount      = $params['amount'];
    $currency    = $params['currency'];
    $description = $params['description'];

    // Client details
    $firstName = $params['clientdetails']['firstname'];
    $lastName  = $params['clientdetails']['lastname'];
    $email     = $params['clientdetails']['email'];

    // System URLs
    $systemUrl  = $params['systemurl'];
    $returnUrl  = $params['returnurl'];
    $callbackUrl = $systemUrl . 'modules/gateways/callback/mygateway.php';

    // Build the payment form
    $htmlOutput  = '<form method="POST" action="https://pay.example.com/checkout">';
    $htmlOutput .= '<input type="hidden" name="merchant_id" value="' . $merchantId . '">';
    $htmlOutput .= '<input type="hidden" name="amount" value="' . $amount . '">';
    $htmlOutput .= '<input type="hidden" name="currency" value="' . $currency . '">';
    $htmlOutput .= '<input type="hidden" name="invoice_id" value="' . $invoiceId . '">';
    $htmlOutput .= '<input type="hidden" name="return_url" value="' . $returnUrl . '">';
    $htmlOutput .= '<input type="hidden" name="callback_url" value="' . $callbackUrl . '">';
    $htmlOutput .= '<input type="hidden" name="email" value="' . $email . '">';
    $htmlOutput .= '<input type="submit" class="btn btn-primary" value="Pay Now">';
    $htmlOutput .= '</form>';

    return $htmlOutput;
}
```

#### Merchant Gateway — Capture Function

```php
<?php

function mygateway_capture(array $params): array
{
    $merchantId = $params['merchantId'];
    $secretKey  = $params['secretKey'];
    $invoiceId  = $params['invoiceid'];
    $amount     = $params['amount'];
    $currency   = $params['currency'];
    $cardNumber = $params['cardnum'];
    $cardExpiry = $params['cardexp'];
    $cardCvv    = $params['cccvv'];

    try {
        $response = mygateway_apiCall('charge', [
            'merchant_id' => $merchantId,
            'amount'      => $amount,
            'currency'    => $currency,
            'card'        => $cardNumber,
            'expiry'      => $cardExpiry,
            'cvv'         => $cardCvv,
            'reference'   => $invoiceId,
        ]);

        logModuleCall('mygateway', 'capture', [
            'amount' => $amount, 'invoice' => $invoiceId,
        ], $response, null, [$cardNumber, $cardCvv, $secretKey]);

        if ($response['status'] === 'approved') {
            return [
                'status'  => 'success',
                'transid' => $response['transaction_id'],
                'rawdata' => $response,
            ];
        }

        return [
            'status'  => 'declined',
            'rawdata' => $response,
        ];
    } catch (\Exception $e) {
        logModuleCall('mygateway', 'capture', $params, $e->getMessage());
        return [
            'status'  => 'error',
            'rawdata' => $e->getMessage(),
        ];
    }
}
```

#### Callback File Template

```php
<?php

// /modules/gateways/callback/mygateway.php

require_once __DIR__ . '/../../../init.php';
require_once __DIR__ . '/../../../includes/gatewayfunctions.php';
require_once __DIR__ . '/../../../includes/invoicefunctions.php';

$gatewayModuleName = basename(__FILE__, '.php');
$gatewayParams = getGatewayVariables($gatewayModuleName);

if (!$gatewayParams['type']) {
    die("Module not activated");
}

// Read and validate the callback data
$transactionId   = $_POST['transaction_id'] ?? '';
$invoiceId       = $_POST['invoice_id'] ?? '';
$transactionAmount = $_POST['amount'] ?? '';
$paymentStatus   = $_POST['status'] ?? '';
$signature       = $_POST['signature'] ?? '';

// Verify the callback signature
$expectedSignature = hash_hmac(
    'sha256',
    $transactionId . $invoiceId . $transactionAmount,
    $gatewayParams['secretKey']
);

if (!hash_equals($expectedSignature, $signature)) {
    logTransaction($gatewayModuleName, $_POST, 'Invalid Signature');
    die("Invalid signature");
}

// Validate invoice ID
$invoiceId = checkCbInvoiceID($invoiceId, $gatewayModuleName);

// Check for duplicate transaction
checkCbTransID($transactionId);

if ($paymentStatus === 'completed') {
    // Add payment to the invoice
    addInvoicePayment(
        $invoiceId,
        $transactionId,
        $transactionAmount,
        0, // Payment fee
        $gatewayModuleName
    );

    logTransaction($gatewayModuleName, $_POST, 'Successful');
} else {
    logTransaction($gatewayModuleName, $_POST, 'Payment Failed: ' . $paymentStatus);
}
```

---

## 6. Action Hooks

### Hook File Locations

| Location                          | Scope                          |
|-----------------------------------|--------------------------------|
| `/includes/hooks/*.php`           | Global hooks (always active)   |
| `/modules/addons/{name}/hooks.php`| Module hooks (when activated)  |
| `/modules/servers/{name}/hooks.php`| Server module hooks           |

> **Important**: If you add `hooks.php` to a module *after* it has been
> activated, you must deactivate and reactivate the module (or re-save
> module settings) for WHMCS to recognise the new hooks file.

### Hook Syntax

```php
<?php

// Using a closure (anonymous function)
add_hook('ClientAdd', 1, function (array $vars) {
    $clientId  = $vars['userid'];
    $firstName = $vars['firstname'];
    $lastName  = $vars['lastname'];
    $email     = $vars['email'];

    logActivity("New client registered: {$firstName} {$lastName} (#{$clientId})");
});

// Using a named function (recommended for complex hooks)
add_hook('InvoicePaid', 1, 'mymodule_hookInvoicePaid');

function mymodule_hookInvoicePaid(array $vars): void
{
    $invoiceId = $vars['invoiceid'];

    // Fetch invoice details via Internal API
    $invoice = localAPI('GetInvoice', ['invoiceid' => $invoiceId]);

    if ($invoice['result'] === 'success') {
        logActivity("Invoice #{$invoiceId} paid. Total: {$invoice['total']}");
    }
}
```

### Priority System

- Priority is an integer. Lower numbers execute first.
- Use `1` for most hooks (default/highest priority).
- Use `10+` for hooks that should run after others.

### Most-Used Hook Points (Quick Reference)

#### Client Lifecycle
| Hook                     | Trigger                             |
|--------------------------|-------------------------------------|
| `ClientAdd`              | A new client account is created     |
| `ClientEdit`             | Client profile is updated           |
| `ClientClose`            | Client account is closed            |
| `ClientDelete`           | Client account is deleted           |
| `ClientChangePassword`   | Client changes their password       |

#### Billing & Invoices
| Hook                     | Trigger                             |
|--------------------------|-------------------------------------|
| `InvoiceCreated`         | A new invoice is created            |
| `InvoicePaid`            | An invoice is marked as paid        |
| `InvoiceRefunded`        | An invoice is refunded              |
| `InvoiceCancelled`       | An invoice is cancelled             |
| `AddInvoicePayment`      | A payment is applied to an invoice  |

#### Module Events
| Hook                     | Trigger                             |
|--------------------------|-------------------------------------|
| `AfterModuleCreate`      | After a service is provisioned      |
| `AfterModuleSuspend`     | After a service is suspended        |
| `AfterModuleUnsuspend`   | After a service is unsuspended      |
| `AfterModuleTerminate`   | After a service is terminated       |
| `PreModuleCreate`        | Before provisioning (can abort)     |

#### Support Tickets
| Hook                     | Trigger                             |
|--------------------------|-------------------------------------|
| `TicketOpen`             | A new ticket is submitted           |
| `TicketAdminReply`       | An admin replies to a ticket        |
| `TicketUserReply`        | A client replies to a ticket        |
| `TicketClose`            | A ticket is closed                  |
| `TicketStatusChange`     | Ticket status changes               |

#### Domain Events
| Hook                     | Trigger                             |
|--------------------------|-------------------------------------|
| `AfterRegistrarRegistration` | Domain registration completes   |
| `AfterRegistrarTransfer` | Domain transfer completes           |
| `AfterRegistrarRenewal`  | Domain renewal completes            |

> **Full Hook Index**: [developers.whmcs.com/hooks/hook-index/](https://developers.whmcs.com/hooks/hook-index/)
> **Hook Reference Manual**: [developers.whmcs.com/hooks-reference/](https://developers.whmcs.com/hooks-reference/)

---

## 7. API Integration

### Internal API (Local API)

Use `localAPI()` when making API calls from **within** the WHMCS runtime
(modules, hooks, or custom pages). No authentication required — the call
runs with admin-level privileges.

```php
<?php

// Example: Get client details
$results = localAPI('GetClientsDetails', [
    'clientid' => $clientId,
    'stats'    => true,
]);

if ($results['result'] === 'success') {
    $clientName = $results['fullname'];
    $email      = $results['email'];
    $status     = $results['status'];
}

// Example: Add a credit
localAPI('AddCredit', [
    'clientid' => $clientId,
    'amount'   => 10.00,
    'description' => 'Welcome credit applied by module',
]);

// Example: Open a ticket
localAPI('OpenTicket', [
    'clientid'   => $clientId,
    'deptid'     => 1,
    'subject'    => 'Account Provisioned',
    'message'    => 'Your account has been provisioned successfully.',
    'priority'   => 'Medium',
]);

// Example: Send an email
localAPI('SendEmail', [
    'messagename' => 'Service Welcome Email',
    'id'          => $serviceId,
]);
```

> **Important**: `localAPI()` does **not** require `init.php` when called
> from within hooks or modules (it's already loaded). Only include
> `init.php` when calling from standalone scripts.

### External API (Remote Calls)

Use `WHMCS\Module\Guzzle` or `GuzzleHttp\Client` (bundled with WHMCS) for
external HTTP requests.

```php
<?php

use GuzzleHttp\Client;
use GuzzleHttp\Exception\RequestException;

function mymodule_apiCall(string $endpoint, array $data): array
{
    $client = new Client([
        'base_uri' => 'https://api.example.com/v1/',
        'timeout'  => 30,
        'headers'  => [
            'Authorization' => 'Bearer ' . $apiKey,
            'Content-Type'  => 'application/json',
            'Accept'        => 'application/json',
        ],
    ]);

    try {
        $response = $client->post($endpoint, [
            'json' => $data,
        ]);

        $result = json_decode($response->getBody()->getContents(), true);

        logModuleCall('mymodule', $endpoint, $data, $result);

        return $result;
    } catch (RequestException $e) {
        $errorResponse = $e->hasResponse()
            ? $e->getResponse()->getBody()->getContents()
            : $e->getMessage();

        logModuleCall('mymodule', $endpoint, $data, $errorResponse);

        throw new \RuntimeException('API call failed: ' . $errorResponse);
    }
}
```

> **API Index**: [developers.whmcs.com/api/api-index/](https://developers.whmcs.com/api/api-index/)
> **API Reference**: [developers.whmcs.com/api-reference/](https://developers.whmcs.com/api-reference/)

---

## 8. Templating & UI

### Smarty Template Rules

- All templates use `.tpl` extension and reside within the module's
  `templates/` directory.
- **Never** use the deprecated `{php}` tag — it is removed in Smarty v4.
- Use `{$variable}` for output and `{if}`, `{foreach}` for logic.
- Escape output with `{$variable|escape:'html'}` for user-generated content.

### Admin Area UI — Native CSS Classes

Use WHMCS built-in Bootstrap and admin CSS classes for a native look:

```html
{* templates/admin/dashboard.tpl *}

<div class="tab-content">

    {* Stats Cards *}
    <div class="row">
        <div class="col-sm-4">
            <div class="panel panel-default">
                <div class="panel-heading">
                    <h3 class="panel-title">Total Clients</h3>
                </div>
                <div class="panel-body text-center">
                    <h1>{$totalClients}</h1>
                </div>
            </div>
        </div>
    </div>

    {* Data Table *}
    <table id="myModuleTable" class="datatable" width="100%">
        <thead>
            <tr>
                <th>ID</th>
                <th>Client</th>
                <th>Status</th>
                <th>Actions</th>
            </tr>
        </thead>
        <tbody>
            {foreach from=$items item=row}
            <tr>
                <td>{$row.id}</td>
                <td>{$row.client_name|escape:'html'}</td>
                <td>
                    <span class="label label-{if $row.status == 'Active'}success{else}danger{/if}">
                        {$row.status}
                    </span>
                </td>
                <td>
                    <a href="{$moduleLink}&action=view&id={$row.id}" class="btn btn-sm btn-default">
                        <i class="fas fa-eye"></i> View
                    </a>
                </td>
            </tr>
            {foreachelse}
            <tr>
                <td colspan="4" class="text-center">No records found.</td>
            </tr>
            {/foreach}
        </tbody>
    </table>

</div>
```

### Client Area UI

```html
{* templates/client/dashboard.tpl *}

<div class="row">
    <div class="col-md-12">
        <h3>{$LANG.module_dashboard_title}</h3>

        {if $data|count > 0}
        <div class="table-responsive">
            <table class="table table-striped table-hover">
                <thead>
                    <tr>
                        <th>{$LANG.column_id}</th>
                        <th>{$LANG.column_name}</th>
                        <th>{$LANG.column_status}</th>
                    </tr>
                </thead>
                <tbody>
                    {foreach from=$data item=item}
                    <tr>
                        <td>{$item.id}</td>
                        <td>{$item.key|escape:'html'}</td>
                        <td>{$item.value|escape:'html'}</td>
                    </tr>
                    {/foreach}
                </tbody>
            </table>
        </div>
        {else}
        <div class="alert alert-info">
            {$LANG.no_records_found}
        </div>
        {/if}
    </div>
</div>
```

---

## 9. Security Checklist

```
☑  WHMCS Access Guard     defined("WHMCS") or die("Access Denied");
☑  Parameter Binding      Use Capsule for all DB queries (auto-binding)
☑  Input Validation       Validate/sanitise all $_POST / $_GET input
☑  CSRF Protection        check_token in POST requests (admin area)
☑  Credential Security    Use encrypt()/decrypt() for stored secrets
☑  API Key Scrubbing      Pass secrets to $replaceVars in logModuleCall()
☑  Output Escaping        Use htmlspecialchars() or Smarty |escape
☑  Error Masking          Log detailed errors; show generic messages to users
☑  File Permissions       644 for files, 755 for directories
☑  No Direct DB Access    Never use mysql_*/mysqli_* — always Capsule
☑  HTTPS Enforcement      Use 'forcessl' => true in client area output
☑  Rate Limiting          Implement API call rate limits where applicable
☑  Signature Validation   Verify HMAC signatures in gateway callbacks
```

---

## 10. Error Handling & Logging

### logModuleCall() — Module Debug Log

Records API interactions viewable in Configuration → System Logs → Module Log.

```php
<?php

logModuleCall(
    'mymodule',           // Module name
    'CreateAccount',      // Action being performed
    $requestData,         // Input parameters (string or array)
    $rawResponse,         // Raw API response
    $processedResponse,   // Processed/decoded response (optional)
    [                     // Sensitive strings to scrub from logs
        $apiPassword,
        $apiKey,
        $cardNumber,
    ]
);
```

### logActivity() — System Activity Log

Records an entry in the WHMCS System Activity Log (visible to admins).

```php
<?php

logActivity("MyModule: Client #{$clientId} provisioned on server {$serverName}");
logActivity("MyModule Error: Failed to connect to API - " . $e->getMessage());
```

### Module Function Error Returns

```php
<?php

// Provisioning modules: return a string
// 'success' on success, error message string on failure
return 'success';
return 'Error: Could not connect to server';

// Registrar modules: return an array
return ['success' => true];
return ['error' => 'Domain not found at registrar'];

// Addon activate/deactivate: return status array
return ['status' => 'success', 'description' => 'Module activated.'];
return ['status' => 'error', 'description' => 'Table creation failed.'];

// Gateway capture: return status array
return ['status' => 'success', 'transid' => $txnId, 'rawdata' => $response];
return ['status' => 'declined', 'rawdata' => $response];
return ['status' => 'error', 'rawdata' => $errorMessage];
```

---

## 11. Module Upgrade Pattern

Use the `_upgrade` function to manage database schema changes between
versions. WHMCS calls this function automatically when the version in
`_config()` is higher than the previously-stored version.

```php
<?php

function mymodule_upgrade(array $vars): void
{
    $currentVersion = $vars['version'];

    // Version 1.1.0: Add status column
    if (version_compare($currentVersion, '1.1.0', '<')) {
        try {
            Capsule::schema()->table('mod_mymodule_data', function ($table) {
                $table->string('status', 50)->default('pending')->after('value');
            });
            logActivity("MyModule: Upgraded to v1.1.0 — added status column.");
        } catch (\Exception $e) {
            logActivity("MyModule: Upgrade to v1.1.0 failed — " . $e->getMessage());
        }
    }

    // Version 1.2.0: Add index and new table
    if (version_compare($currentVersion, '1.2.0', '<')) {
        try {
            Capsule::schema()->table('mod_mymodule_data', function ($table) {
                $table->index('status');
            });

            if (!Capsule::schema()->hasTable('mod_mymodule_audit')) {
                Capsule::schema()->create('mod_mymodule_audit', function ($table) {
                    $table->increments('id');
                    $table->unsignedInteger('record_id');
                    $table->string('action', 100);
                    $table->text('details')->nullable();
                    $table->timestamp('created_at')->useCurrent();
                });
            }
            logActivity("MyModule: Upgraded to v1.2.0 — added audit table.");
        } catch (\Exception $e) {
            logActivity("MyModule: Upgrade to v1.2.0 failed — " . $e->getMessage());
        }
    }
}
```

---

## 12. Common Pitfalls & Anti-Patterns

### 🔴 Critical Issues (Will Break Your Module)

| Pitfall                                  | Why It Breaks                                                    | Fix                                                              |
|------------------------------------------|------------------------------------------------------------------|------------------------------------------------------------------|
| Using `mysql_*` / `mysqli_*`             | Removed/deprecated; breaks on PHP 8+                             | Use `Capsule::table()`                                           |
| Using `{php}` in Smarty templates        | Removed in Smarty v4 (WHMCS 9.x)                                | Use Smarty tags or pass data via `vars`                          |
| Dynamic properties in classes            | Deprecated in PHP 8.2, error in 9.0                              | Declare all properties explicitly                                |
| Missing `defined("WHMCS")` guard         | Files become directly accessible via URL                         | Add guard as first line of every PHP file                        |
| Hardcoding file paths                    | Breaks across installations/environments                         | Use `ROOTDIR` and WHMCS constants                                |
| Modifying core files                     | Overwritten on WHMCS updates, causes corruption                  | Use Hooks and Modules instead                                    |
| Returning instead of echoing in `_output`| Admin output function must `echo`, not `return`                  | Use `echo` in addon `_output()`                                  |
| Echoing in `_clientarea`                 | Client area function must `return` an array                      | Return `['templatefile' => ..., 'vars' => ...]`                  |

### 🟡 Common Mistakes (Will Cause Bugs)

| Pitfall                                  | Issue                                                            | Fix                                                              |
|------------------------------------------|------------------------------------------------------------------|------------------------------------------------------------------|
| Not wrapping DB calls in try/catch       | Unhandled exceptions crash the admin area                        | Always use try/catch with Capsule operations                     |
| Forgetting `logModuleCall()`             | Impossible to debug API issues in production                     | Log every external API call                                      |
| Not scrubbing sensitive data from logs   | Passwords/keys visible in Module Log                             | Pass sensitive strings in `$replaceVars`                         |
| Raw SQL table names without `tbl` prefix | WHMCS core tables use `tbl` prefix (e.g., `tblclients`)         | Use exact table names including prefix                           |
| Adding hooks.php after module activation | WHMCS won't recognise new hooks file                             | Deactivate and reactivate module                                 |
| Using `$_REQUEST` instead of specific    | Security risk; ambiguous data source                             | Use `$_POST` or `$_GET` explicitly                               |
| Forgetting language files                | Module is not translatable; hard-coded strings                   | Create `lang/english.php` with all strings                       |
| Not using the `_upgrade` function        | Schema changes not applied when users update the module          | Use `version_compare()` in `_upgrade()`                          |
| Illuminate v7 patterns on WHMCS 9.x     | Fatal errors from incompatible Eloquent methods                  | Test queries specifically against Illuminate v9                  |
| Not validating callback signatures       | Gateway callbacks can be forged                                  | Always verify HMAC signatures                                    |

### 🟢 Best Practices (Will Make Users Love Your Module)

| Practice                                 | Benefit                                                          |
|------------------------------------------|------------------------------------------------------------------|
| Use WHMCS native Bootstrap classes       | Module UI looks native and professional                          |
| Include comprehensive language files     | Module is instantly translatable for international users         |
| Implement `TestConnection` (provisioning)| Admin can verify server connectivity before using the module     |
| Include breadcrumbs in client area       | Better navigation UX for clients                                 |
| Use `_upgrade` for all schema changes    | Seamless version upgrades without manual SQL                     |
| Bundle a README.md with the module       | Users know how to install and configure                          |
| Add an Admin Dashboard Widget            | Key metrics visible at a glance                                  |
| Support configurable options via _config | No code changes needed for different environments                |

---

## 13. Project Structure Templates

### Addon Module Structure

```
/modules/addons/mymodule/
├── mymodule.php              # Main file: _config, _activate, _deactivate,
│                             #   _upgrade, _output, _clientarea
├── hooks.php                 # Action hooks (auto-loaded when module active)
├── lang/
│   └── english.php           # Language strings
├── lib/
│   ├── ApiClient.php         # External API client class
│   ├── Helper.php            # Utility functions
│   └── DataManager.php       # Database abstraction layer
├── templates/
│   ├── admin/
│   │   ├── dashboard.tpl     # Admin dashboard view
│   │   ├── settings.tpl      # Admin settings view
│   │   └── list.tpl          # Admin data listing view
│   └── client/
│       └── dashboard.tpl     # Client area dashboard view
├── README.md                 # Installation & usage guide
├── CHANGELOG.md              # Version history
└── LICENSE                   # License file
```

### Provisioning Module Structure

```
/modules/servers/myserver/
├── myserver.php              # Main file: all provisioning functions
├── hooks.php                 # Action hooks (optional)
├── lib/
│   ├── ApiClient.php         # Server API client class
│   └── Helper.php            # Utility functions
├── templates/
│   └── clientarea.tpl        # Client area service output
├── README.md
└── CHANGELOG.md
```

### Registrar Module Structure

```
/modules/registrars/myregistrar/
├── myregistrar.php           # Main file: all registrar functions
├── hooks.php                 # Action hooks (optional)
├── lib/
│   ├── ApiClient.php         # Registrar API client class
│   └── DomainHelper.php      # Domain utility functions
├── README.md
└── CHANGELOG.md
```

### Payment Gateway Structure

```
/modules/gateways/
├── mygateway.php             # Main gateway file (_config, _link or _capture)
└── callback/
    └── mygateway.php         # Callback handler for payment notifications

/modules/gateways/mygateway/  # (Optional) Library files
├── lib/
│   └── ApiClient.php
└── README.md
```

---

## 14. Quick-Reference Code Snippets

### Module Security Guard

```php
<?php
// FIRST LINE of every PHP file in a module
defined("WHMCS") or die("Access Denied");
```

### Language File Template

```php
<?php
// /modules/addons/mymodule/lang/english.php

$_ADDONLANG['module_title']           = 'My Module';
$_ADDONLANG['dashboard_title']        = 'Dashboard';
$_ADDONLANG['settings_title']         = 'Settings';
$_ADDONLANG['client_page_title']      = 'My Module';
$_ADDONLANG['no_records_found']       = 'No records found.';
$_ADDONLANG['column_id']             = 'ID';
$_ADDONLANG['column_name']           = 'Name';
$_ADDONLANG['column_status']         = 'Status';
$_ADDONLANG['btn_save']              = 'Save Changes';
$_ADDONLANG['btn_cancel']            = 'Cancel';
$_ADDONLANG['success_saved']         = 'Settings saved successfully.';
$_ADDONLANG['error_generic']         = 'An error occurred. Please try again.';
```

### Capsule Check If Table Exists

```php
<?php

use Illuminate\Database\Capsule\Manager as Capsule;

if (Capsule::schema()->hasTable('mod_mymodule_data')) {
    // Table exists
}

if (Capsule::schema()->hasColumn('mod_mymodule_data', 'status')) {
    // Column exists
}
```

### Fetch Admin User in Module

```php
<?php

// Inside _output() — get current admin user ID
$adminId = $_SESSION['adminid'];
$admin = Capsule::table('tbladmins')->where('id', $adminId)->first();
```

### Client Area URL Routing

```php
<?php

// Client area modules are accessed at:
// https://yourdomain.com/index.php?m=mymodule
// https://yourdomain.com/index.php?m=mymodule&action=settings

function mymodule_clientarea($vars): array
{
    $action = $_GET['action'] ?? 'index';

    switch ($action) {
        case 'settings':
            return [
                'pagetitle'    => 'Settings',
                'templatefile' => 'templates/client/settings',
                'requirelogin' => true,
                'vars'         => ['currentSettings' => $settings],
            ];

        default:
            return [
                'pagetitle'    => 'Dashboard',
                'templatefile' => 'templates/client/dashboard',
                'requirelogin' => true,
                'vars'         => ['data' => $data],
            ];
    }
}
```

### Hook — Add Menu Entry

```php
<?php

add_hook('ClientAreaPrimarySidebar', 1, function ($sidebar) {
    /** @var \WHMCS\View\Menu\Item $sidebar */
    $myPanel = $sidebar->addChild('myModulePanel', [
        'label' => 'My Module',
        'uri'   => 'index.php?m=mymodule',
        'order' => 99,
        'icon'  => 'fas fa-cog',
    ]);

    $myPanel->addChild('dashboard', [
        'label' => 'Dashboard',
        'uri'   => 'index.php?m=mymodule',
        'order' => 1,
    ]);

    $myPanel->addChild('settings', [
        'label' => 'Settings',
        'uri'   => 'index.php?m=mymodule&action=settings',
        'order' => 2,
    ]);
});
```

### Hook — Modify Checkout

```php
<?php

add_hook('ShoppingCartValidateCheckout', 1, function (array $vars) {
    $errors = [];

    // Example: require a custom field to be filled
    if (empty($_POST['customfield_company'])) {
        $errors[] = 'Company name is required for checkout.';
    }

    // Return errors to block checkout, or empty array to proceed
    return $errors;
});
```

### WHMCS Encryption Helpers

```php
<?php

// Encrypt sensitive data before storing
$encrypted = encrypt('my-secret-api-key');
Capsule::table('mod_mymodule_settings')
    ->insert(['key' => 'api_key', 'value' => $encrypted]);

// Decrypt when reading
$row = Capsule::table('mod_mymodule_settings')
    ->where('key', '=', 'api_key')
    ->first();
$apiKey = decrypt($row->value);
```

---

## 15. Official References

| Resource                            | URL                                                          |
|-------------------------------------|--------------------------------------------------------------|
| **Developer Documentation (Home)**  | https://developers.whmcs.com/                                |
| **API Reference**                   | https://developers.whmcs.com/api-reference/                  |
| **API Index**                       | https://developers.whmcs.com/api/api-index/                  |
| **Hook Reference**                  | https://developers.whmcs.com/hooks-reference/                |
| **Hook Index**                      | https://developers.whmcs.com/hooks/hook-index/               |
| **Classes Reference**               | https://docs.whmcs.com/classes/                              |
| **Style Guide**                     | https://developers.whmcs.com/modules/style-guide/            |
| **Sample Addon Module**             | https://github.com/WHMCS/sample-addon-module                 |
| **Sample Provisioning Module**      | https://github.com/WHMCS/sample-provisioning-module          |
| **Sample Registrar Module**         | https://github.com/WHMCS/sample-registrar-module             |
| **Sample Gateway Module**           | https://github.com/WHMCS/sample-gateway-module               |
| **Sample Merchant Gateway**         | https://github.com/WHMCS/sample-merchant-gateway             |
| **WHMCS Community Forums**          | https://whmcs.community/                                     |
| **Beta API Documentation**          | https://api-beta.developers.whmcs.com/                       |

---

## License

This skill is licensed under the [GNU General Public License v2.0](https://www.gnu.org/licenses/old-licenses/gpl-2.0.en.html).
You may use, modify, and redistribute it under the terms of GPL-2.0.

---

## Author

**Waqas Ahmed Waseer**

- 🌐 Website: [waqasahmedwaseer.com](https://waqasahmedwaseer.com)
- 🐙 GitHub: [@waqasahmedwaseer](https://github.com/waqasahmedwaseer)
- 📦 Skill: `whmcs-dev-skills`

---

> *Built with ❤️ for the WHMCS developer community.*
> *Researched from official WHMCS Developer Documentation, common issue reports,*
> *and community best practices (2024–2025).*
