## Create custom folder

Assume that you are using version 2.0.3. Create a folder `custom` under `seafile-server-2.0.3/seahub/media`. Put all the customization files here. The upgrade script will copy the folder to `seafile-server-2.0.4/seahub/media` when you upgrade to version 2.0.4.

## Customize Seahub Logo

1. Add your logo file to `seahub/media/custom/`

2. Overwrite `LOGO_PATH` in `seahub_settings.py`

<pre>
LOGO_PATH = 'custom/mylogo.png'
</pre>

3. Overwrite `LOGO_URL` in `seahub_settings.py`

<pre>
LOGO_URL = 'http://your-seafile.com'
</pre>

## Custmize Seahub CSS

1. Add your css file to `seahub/media/custom/`, for example, `custom.css`

2. Overwrite `BRANDING_CSS` in `seahub_settings.py`

<pre>
BRANDING_CSS = 'custom/custom.css'
</pre>