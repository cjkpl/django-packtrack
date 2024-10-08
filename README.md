# Outdated Package Tracker

> :warning: **Use at your own risk.**. Author is not responsible for any damage resulting from using this application.

### Show and update outdated versions of python packages in your project.

The app serves to:
- list in django admin all outdated python packages
- show dependent apps using each package
- locally update them one-by-one using django backend
- optionally, exposes via DRF an endpoint to ```PackHub``` application to allow remotely querying the list of outdated packages

## Installation

```
pip install django-packtrack
```

## Installed apps and dependencies

```python
    # add to settings, INSTALLED_APPS:
    "packtrack",
    # dependencies for packtrack: 
    "oauth2_provider",
    "rest_framework",  # packtrack, packhub
    "rest_framework_simplejwt",  # packtrack
```

## Configuration

In the project settings, add ```REST_FRAMEWORK``` entry if not present, and optionally ```PACKTRACK_LATEST_VERSION_CACHE_TIME``` to modify the default 1 hour

```python
PACKTRACK_LATEST_VERSION_CACHE_TIME = timedelta(
    hours=1
)  # optional entry, default is 1 hour

REST_FRAMEWORK = {
    "DEFAULT_AUTHENTICATION_CLASSES": (
        "oauth2_provider.contrib.rest_framework.OAuth2Authentication",  # for clientid/secret authentication
    ),
    "DEFAULT_PERMISSION_CLASSES": ("rest_framework.permissions.IsAuthenticated",),
}
```

## CLI Usage
These commands are designed to be used from within the application interface, or via DRF, but if you really want to call them from CLI (rather than directly calling ```pip```), suit yourself :-)

The following management commands are available:

### ```update_package {package_name}```

Executes ```pip``` command updating the package to the most current version, as stored in PackageCache

## Django Admin /UI usage

In Django Admin, under ```PackTrack``` app, in the ```Outdated packages```:
- at the top right, hit "Updated Outdated Packages" button to refresh the list of outdated packages in your project 
- in the list below, you can update each package by individually by clicking the "Update" button.
- to hide the updated packages, use the filter with option "Installed != Latest", to show only old packages.

In the "Update Logs" you can see what was updated and when, and you can examine the console outputs from each update.

## API usage
1. Configure Application in Django Oauth Toolkit, to get your Client ID, and Client Secret, e.g.:
 - client type: Confidential
 - Authorization grant type: Client credentials
 - Hash client secret
2. You can then fetch the outdated packages from a site where PackTrack is running using ```/api/outdated-packages/``` endpoint.