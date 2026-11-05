

## Przykład 1

```csharp
using System.Net.Mail;

public class NewsletterService
{
    public void SendUpdate(string userEmail, string newsContent)
    {
        // BŁĄD: Twarda zależność od klienta SMTP
        SmtpClient client = new SmtpClient("smtp.naszserwer.pl");
        MailMessage mail = new MailMessage("no-reply@szkola.pl", userEmail);
        mail.Subject = "Nowości";
        mail.Body = newsContent;

        client.Send(mail); // Prawdziwy mail poleci w świat podczas testu!
    }
}
```

## Przykład 2

```csharp
using System.Net.Http;
using System.Threading.Tasks;

public class CurrencyConverter
{
    public async Task<decimal> ConvertPlnToUsd(decimal plnAmount)
    {
        // BŁĄD: Bezpośrednie użycie HttpClient i połączenie z Internetem
        using var client = new HttpClient();
        string response = await client.GetStringAsync("https://api.nbp.pl/api/exchangerates/rates/a/usd/?format=json");
        
        // Uproszczone parsowanie (w rzeczywistości użyto by JSON.NET)
        decimal exchangeRate = ParseRateFromJson(response); 
        
        return plnAmount / exchangeRate;
    }

    private decimal ParseRateFromJson(string json) => 4.0m; // atrapa parsowania
}
```

## Przykład 3

```csharp
using System;

public class LoginManager
{
    public bool Authenticate(string username, string password)
    {
        // BŁĄD: Instancjonowanie konkretnej bazy danych i szukanie w niej użytkownika
        var db = new SqlDatabaseContext("Server=myServer;Database=myDB;");
        
        var user = db.Users.Find(username);
        
        if (user == null) return false;
        
        if (user.IsLocked) throw new UnauthorizedAccessException("Konto zablokowane!");

        return user.PasswordHash == Hash(password);
    }

    private string Hash(string pass) => pass + "_hashed"; // Uproszczenie
}
```

## Przykład 4

```csharp
using System.IO;
using System.IO.Compression;

public class BackupService
{
    public void CreateDailyBackup(string sourceFolder)
    {
        string backupName = $"backup_{DateTime.Now:yyyyMMdd}.zip";
        string destinationPath = @"C:\Backups\" + backupName;

        // BŁĄD: Operacje na dysku twardym systemu Windows.
        if (Directory.Exists(sourceFolder))
        {
            ZipFile.CreateFromDirectory(sourceFolder, destinationPath);
        }
    }
}
```

## Przykład 5

```csharp
using System;
using System.IO;

public class CryptoWallet
{
    public decimal Balance { get; private set; } = 100m;

    public bool TransferFunds(string targetWalletAddress, decimal amount)
    {
        if (amount > Balance)
        {
            // BŁĄD: Zapis do pliku logów wewnątrz logiki biznesowej
            File.AppendAllText("error_log.txt", "Próba przelewu bez środków.");
            return false;
        }

        // BŁĄD: Powiązanie z konkretną usługą zewnętrzną w sieci
        var blockchainClient = new EthereumNetworkClient();
        bool success = blockchainClient.SendTransaction(targetWalletAddress, amount);

        if (success)
        {
            Balance -= amount;
        }
        return success;
    }
}
```

## Przykład 6

```csharp
using System;

public class SmartCoffeeMaker
{
    public void CheckAndBrew()
    {
        // BŁĄD: Twarda zależność od zegara systemowego i fizycznego sprzętu
        if (DateTime.Now.Hour == 7)
        {
            var hardware = new CoffeeMachineHardware();
            hardware.StartBrewing();
        }
    }
}
```

## Przykład 7

```csharp
using System;

public class TwoFactorAuth
{
    public void SendAuthCode(string phoneNumber)
    {
        // BŁĄD: Wewnętrzna losowość i twarde wywołanie bramki SMS
        var random = new Random();
        int code = random.Next(1000, 9999);
        
        var smsApi = new SmsGatewayApi();
        smsApi.Send(phoneNumber, $"Twój kod to: {code}");
    }
}
```

## Przykład 8

```csharp
using System.IO;

public class DocumentUploader
{
    public bool UploadFile(string filePath)
    {
        // BŁĄD: Bezpośrednie odpytywanie fizycznego dysku
        var fileInfo = new FileInfo(filePath);
        
        // Jeśli plik waży więcej niż 10 MB (10 485 760 bajtów)
        if (fileInfo.Length > 10485760)
        {
            return false;
        }

        // ... logika wysyłania (pominięta) ...
        return true;
    }
}
```

## Przykład 9

```csharp
public class FireAlarmSystem
{
    public void CheckEnvironment()
    {
        // BŁĄD: Ścisłe powiązanie z fizycznym czujnikiem i systemami budynku
        var smokeDetector = new HardwareSmokeDetector();
        int smokeLevel = smokeDetector.ReadSmokeLevel();

        if (smokeLevel > 50)
        {
            new AlarmSiren().SoundAlarm();
            new SprinklerSystem().Activate();
        }
    }
}
```

## Przykład 10

```csharp
public class OrderCheckout
{
    public bool ProcessOrder(string cardNumber, decimal amount)
    {
        // BŁĄD: Wywołanie statycznej metody płatności zewnętrznej usługi (np. Stripe/PayPal)
        bool paymentSuccess = ExternalPaymentGateway.ChargeCard(cardNumber, amount);

        if (paymentSuccess)
        {
            var db = new Database();
            db.SaveOrder(amount);
            return true;
        }

        return false;
    }
}
```
