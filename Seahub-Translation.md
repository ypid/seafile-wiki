
## Translate ##

1. Locate the translation file in your language. For example, if you want to translate Russian, the file is seahub/locale/ru/LC_MESSAGES/django.po

2. Open the file using utf-8 text editor.

3. Edit django.po, and save your modification.

4. Run command under seahub/locale/<lang-code>/LC_MESSAGES

  msgfmt -o django.mo django.po

5. Restart seahub

**Note:** If the file in your language does not exists, please contact us, we will create that file for you.

## Submit Translation ##

Please submit translations via Transifex: https://www.transifex.com/projects/p/seahub/
