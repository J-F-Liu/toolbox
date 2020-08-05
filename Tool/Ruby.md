# Ruby

## Install
```
pacman -S ruby
```

## Update gem
```
gem sources --remove https://rubygems.org/
gem sources -a https://gems.ruby-china.com/
gem sources -l

gem update --system
gem install bundler
```

## Rake
```
rake -T -A 列出所有的task
rake <task> 执行某个task
```

## Use specific ruby version

```
yay -S rbenv ruby-build
```