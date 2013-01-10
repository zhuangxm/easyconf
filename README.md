# Easyconf

A config library for clojure program.

* 2012-4-17 Release 0.1.1

## Usage

1. add to dependencies section of porject.clj

   ```clojure
    [easyconf "0.1.1"]
   ```

1. Define variables that can be configured at runtime
under easyconf.core namespace, using macros defconf or
defnconf.  These two macro just like def and defn.

   ```clojure
    (defconf ^{:validator string? :validator-msg "configure value must be
       string" :conf-name "conf-string" } conf-item "default value"}

    (defnconf game-name [game] (str "game name : " game))
   ```
   
1. Optional meta-data
   Meta :validator :validator-msg :conf-name are optional.
   * :validator  => A predict function which has one argument for the value of the item.
   * :validator-msg => The message which will print when an illegal value provided for the item. 
   * :conf-name => ??

1. In config file, you can redefine the value of configuable item. 

   ```clojure
    (config :conf-string "changed config value")
    (config-once :conf-string "changed config value")

    ;;the code below will trigger a exception
    (config-once :conf-string "change config value twice")  
   ```

## Recommondation

Supply all the config value in a seperated namespace.

  ```clojure
    (ns config.autoload
     (:use [easyconf.core]))

    (config-once :conf-1 "conf-1")
    (config-once :conf-two "conf-two")
    (config-once :game-name (fn [id] (get {"ddz" "doudizhu"} id)))
   ```

## Checking

Function easyconf.confs/check checks if all the config value be used. You should invoke this 
function after the bootstrap of the program.
