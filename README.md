# Google Drive API v3 - Downloading files in PHP

A Quickstart PHP script to download file(s) shared on Google Drive to your local system. You would need to provide the Google file id and filename in an array.  


## Prerequisites

To run this quickstart, you need the following prerequisites:

PHP 5.4 or greater and JSON extension installed

The Composer dependency management tool

A Google account with Google Drive enabled

## Steps

Step 1: Turn on the Drive API

Step 2: Install the Google Client Library

```bash
composer require google/apiclient:^2.0
```

See the library's [installation page](https://developers.google.com/api-client-library/php/start/installation) for the alternative installation options.

Step 3: Set up the sample

Create a PHP file and copy in the following code:

## PHP Code

```php
$time_start = microtime(true);
require __DIR__ . '/vendor/autoload.php';

putenv('GOOGLE_APPLICATION_CREDENTIALS=path/to/folder/key.json');
$client = new Google_Client();
$client->addScope(Google_Service_Drive::DRIVE);
$client->useApplicationDefaultCredentials();

$service = new Google_Service_Drive($client);

$files2Download = array(
	"FILENAME.jpg" => "GOOGLE_FILE_ID",
	...
	...
);
echo "<pre>";
foreach ($files2Download as $fileName => $fileId) {
	$content = $service->files->get($fileId, array("alt" => "media"));
	if ($content){
		// Open file handle for output.
		$outHandle = fopen("path/to/folder/". $fileName, "w+");
		if ($outHandle){
			echo "Downloading ".$fileName." (".$fileId.") ";
			// Until we have reached the EOF, read 1024 bytes at a time and write to the output file handle.
			while (!$content->getBody()->eof()) {
			        fwrite($outHandle, $content->getBody()->read(1024));
			}
			// Close output file handle.
			fclose($outHandle);
		}
		echo "&#10003;";
		echo "\n";
	}
	flush();
	ob_flush();
}
$time_end = microtime(true);
$time = $time_end - $time_start;
echo "Script executed in ".number_format($time, 6). " seconds";
echo "</pre>";
```

## Output

![Sample Output](https://raw.githubusercontent.com/sunilnanda/GoogleDriveDownloadFiles/main/output.gif "Sample Output")

## Useful Links

- [Google Developers Console help documentation](https://developers.google.com/console/help/new)

- [Google APIs Client for PHP documentation](https://developers.google.com/api-client-library/php)

- [Drive API PHP reference documentation](https://developers.google.com/resources/api-libraries/documentation/drive/v3/php/latest)

- [Drive REST API reference documentation](https://developers.google.com/drive/api/v3/reference)


## Contributing
Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

Please make sure to update tests as appropriate.

## License
[GNU General Public License v3.0](https://www.gnu.org/licenses/gpl-3.0.en.html)
