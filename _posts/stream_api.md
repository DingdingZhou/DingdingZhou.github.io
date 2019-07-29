#  新人启动 stream_api 项目过程记录 

1. 下载运行时 runtime
mkdir -p ~/repos/i18nws
cd ~/repos/i18nws
git clone gitr:toutiao/runtime 
 ./runtime/update.sh

2. 更新工具包(如果是生产RPC是必须的)
先更新i18nws/runtime目录：cd ~/repos/i18nws/runtime && git pull --rebase
再更新rpc工具目录：cd ~/repos/i18nws/tools/rpc-tool && git pull --rebase

3. 下载主项目 stream_api
cd ~/repos/i18nws/app
git clone ssh://zhoudingding@git.byted.org:29418/i18n_web/stream_api

4. 下载 runtime_lib
注意，这个名称不一样
cd ~/repos/i18nws/lib/
git clone ssh://zhoudingding@git.byted.org:29418/i18n/web/runtime_lib i18n_runtime_lib

5. 运行项目
cd ~/repos/i18nws/app/stream_api
~/repos/i18nws/runtime/bin/python bootstrap.py

6. 运行出错，补全包
cd ~/repos/i18nws/app/
git clone git@code.byted.org:dp/basic_user_data




