# ConfigMasterIOS
Projeto para Para Guardar as configurações para o MacOS 



###-tns-completion-start-###
if [ -f /Users/thiagogoncalves/.tnsrc ]; then 
    source /Users/thiagogoncalves/.tnsrc 
fi
if [ -f “$(brew --prefix bash-git-prompt)/share/gitprompt.sh” ]; then
   GIT_PROMPT_THEME=Default
   source “$(brew --prefix bash-git-prompt)/share/gitprompt.sh”
fi

if [ -f $(brew --prefix)/etc/bash_completion ]; then
   . $(brew --prefix)/etc/bash_completion
fi

if [ -f ~/.bashrc ]; then
  source ~/.bashrc
fi
###-tns-completion-end-###

export ANDROID_HOME=/Users/thiagogoncalves/Library/Android/sdk
export PATH=/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/Users/thiagogoncalves/Library/Android/sdk/platform-tools/:/tools:/platform-tools
export PATH=$PATH:$ANDROID_HOME/bin:$ANDROID_HOME/tools:$ANDROID_HOME/platform-tools:$ANDROID_HOME/lib:$ANDROID_HOME/tools/lib:$ANDROID_HOME/bin
export PATH=$PATH:/Users/thiagogoncalves/Library/Android/sdk/platform-tools/

eval "$(rbenv init -)"

parse_git_branch() {
     git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/ (\1)/'
}
export PS1="\u@\h \[\033[32m\]\w\[\033[33m\]\$(parse_git_branch)\[\033[00m\] $ "
export CLICOLOR=1
export LSCOLORS=ExFxBxDxCxegedabagacad

##Alias

alias ls='ls -GFh'
alias Go_Itau_Dev=Itau_Dev
alias Go_Itau_Android=Itau_Android
alias Go_Itau_Spec=Itau_Spec
alias Go_Itau_IOS=Itau_IOS
alias Compilar_Android_Dev=Android_Dev
alias Compilar_Android_Mock=Android_Mock
alias Compilar_IOS_Build=IOS_Build
alias Calabash_Android_Console=Android_Console
alias Calabash_Android_run=Android_run
alias Calabash_IOS_Console=IOS_Console
alias Calabash_IOS_run=IOS_run
alias Git_Itau_Update=Git_update_all


##Func Pasta
Itau_Dev() {
	cd $HOME_PATH
	cd dev/
}

Itau_Android() {
	Itau_Dev
	cd android/
}

Itau_Spec(){
	Itau_Dev
	cd spec/
}

Itau_IOS() {
	Itau_Dev
	cd ios/
}

## Compilar Android
Android_Dev() {
	Itau_Android
	./gradlew assembleMobileEmpresasDebug
}

Android_Mock() {
	Itau_Android
	./gradlew assembleMobileEmpresasCs
}


## Calabash Android
Android_Console(){
	Itau_Spec
	bundle exec calabash-android console $HOME/dev/android/app/build/outputs/apk/app-mobileEmpresas-debug.apk -p android-int $1
}

Android_run(){
	Itau_Spec
	bundle exec calabash-android run  $HOME/dev/android/app/build/outputs/apk/app-mobileEmpresas-debug.apk -p android-mock $1
}

##Build IOS
IOS_Build() {
	Itau_Spec
	ruby config/scripts/ios/build_app.rb thiago simulator $1
}

## Calabash IOS

IOS_Console() {
	Itau_Spec
	APP_BUNDLE_PATH=/Users/thiagogoncalves/dev/gif/build/mock/simulator/Empresas.app DEVICE_TARGET=313CA3A9-1ED0-4626-86A4-179199B15A56 bundle exec calabash-ios console -p ios-mock -p ios features $1
}

IOS_run() {
	Itau_Spec
	APP_BUNDLE_PATH=$HOME/dev/gif/build/mock/simulator/Empresas.app DEVICE_TARGET=313CA3A9-1ED0-4626-86A4-179199B15A56 bundle exe cucumber -p ios -p ios-mock  features $1
}


## Git
Git_update_all(){
	Itau_Android
	git checkout
	git pull

	Itau_IOS
	git checkout
	git pull

	Itau_Spec
	git checkout
	git pull
}
