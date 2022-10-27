# Docker_Flask_Sample
Docker + Flask + Nginx + MySQL + Gunicorn


# 1.docker-compose.ymlと同じ階層に「Django_App」ディレクトリを作成
# 2.docker-compose.ymlと同じ階層に「.env」ファイルを作成し記述

# 3.プロジェクトを新規作成
```

# 例：
docker-compose run app django-admin startproject [プロジェクト名] .
```



# 4. settings.pyを編集
```
from pathlib import Path
# osのモジュールをインポート
import os

# [・・・]

# SECRET_KEYを.envから取得
SECRET_KEY = os.environ.get("SECRET_KEY")

# DEBUGを.envから取得
DEBUG = os.environ.get("DEBUG")

# ALLOWED_HOSTSを.envから取得
ALLOWED_HOSTS = os.environ.get("DJANGO_ALLOWED_HOSTS").split(" ")

# [・・・]

# MySQLのパラメータを.envから取得
DATABASES = {
    "default": {
        "ENGINE": "django.db.backends.mysql",
        # コンテナ内の環境変数をDATABASESのパラメータに反映
        "NAME": os.environ.get("MYSQL_DATABASE"),
        "USER": os.environ.get("MYSQL_USER"),
        "PASSWORD": os.environ.get("MYSQL_PASSWORD"),
        "HOST": os.environ.get("MYSQL_HOST"),
        "PORT": os.environ.get("MYSQL_PORT"),
    }
}


# [・・・]

# 言語を日本語に設定
LANGUAGE_CODE = 'ja'
# タイムゾーンをAsia/Tokyoに設定
TIME_ZONE = 'Asia/Tokyo'

# [・・・]

# STATIC_ROOTを設定
# Djangoの管理者画面にHTML、CSS、Javascriptが適用されます
STATIC_ROOT = "/static/"
STATIC_URL = "/static/"
```


# 5.一旦Dockerをリセットする
```
docker-compose down -v

docker rm $(docker ps -a -q)

docker rmi $(docker images -q)

docker system prune
```

# 6.Dockerを起動
```
docker-compose up -d --build
```

# 7.ブラウザで確認
```
http://localhost:8000
```

# 8.Django_App内で環境を作って開発して行く
# 9.そこからappを作成して下さい