## Translate ##

1. Locate the translation file(``seahub/locale/<lang-code>/LC_MESSAGES/django.po``).

  For example, if you want to translate Russian, the file is seahub/locale/ru/LC_MESSAGES/django.po. If the path does not exist, please copy from an exist path, and change ``lang-code`` to fit your language.

2. Open the file using utf-8 text editor.

3. Edit django.po, and save your modification.

4. Run command under seahub/locale/<lang-code>/LC_MESSAGES

  `msgfmt -o django.mo django.po`

5. Change ``LANGUAGE`` settings (optional).

  Open ``seahub/seahub/settings.py``, add a new entry to ``LANGUAGE`` setting if your language is not pre-defined.

6. Restart seahub

## Submit Translation ##

Please submit translations via Transifex: https://www.transifex.com/projects/p/seahub/

Steps:

1. Create a free account on Transifex (https://www.transifex.com/).

2. Send a request to join the language translation.

3. After accepted by the project maintainer, then you can upload your file or translate online.
