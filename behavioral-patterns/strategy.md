# Strategy

* Define the interface of an interchangeable family of algorithms
* Bury algorithm implementation details in derived classes
* Derived classes could be implemented using the Template Method patter
* Clients of the algorithm couple themselves strictly to the interface

### Example - Checking data providers

We are goint to use Strategy pattern to implement various strategies to check websites like Google, eBay and so on.

```
interface Strategy {
    void solve();
}

class GoogleStrategy implements Strategy {
    @Override
    public void solve() {
        System.out.println("Checking Google");
    }
}

class EbayStrategy implements Strategy {
    @Override
    public void solve() {
        System.out.println("Checking eBay");
    }
}

class StrategyExecutor {
    void execute() {
        Strategy[] strategies = {new GoogleStrategy(), new EbayStrategy()};
        Arrays.stream(strategies).forEach(Strategy::solve);
    }
}
```

Then we use the executor to run our strategies.

```
new StrategyExecutor().execute();
```

Here is the output.

```
Checking Google
Checking eBay
```

### Using Strategy to replace complex logic in single class

Lets consider we have create a class that is doing a simple thing. It will acquire an item from a storage. Then we did many modification in time and the class become really complex. Such a class is then difficult to modify and test. Also it is difficult to easily get an idea what is the class actually doing. 

    const _ = require('lodash')

    Q.Errors.declareError('NoProxyAvailableError')

    module.exports = class {
      constructor(
        domainRedisService = 'redis:domainRedisService',
        proxyRedisService = 'redis:proxyRedisService',
        providerService,
        policyService,
        proxyPolicyMongoService = 'mongo:proxyPolicyMongoService',
        policyRedisService = 'redis:policyRedisService',
        lockFactory
      ) {
        this.domainRedisService = domainRedisService
        this.proxyRedisService = proxyRedisService
        this.providerService = providerService
        this.policyMongoService = policyService
        this.proxyPolicyMongoService = proxyPolicyMongoService
        this.policyRedisService = policyRedisService
        this.lockFactory = lockFactory
      }

      async tryToAcquireProxy(domain) {
        await this.tryToCreateProxySet()
        await this.tryToCreateDefaultDomainSet()

        const domainExists = await this.domainRedisService.domainExists(domain)
        if (!domainExists) {
          await this.tryToCreateDomain(domain)
        }
        const proxy = await this.acquireProxy(domain)

        Q.log.info({ domain, proxy }, 'Acquired proxy based on origin or host')
        return proxy
      }

      async acquireProxy(domain) {
        const proxyIndex = await this.domainRedisService.getProxyIndex(domain)
        if (!proxyIndex) {
          throw new Q.Errors.NoProxyAvailableError('Not able to find proxy index for domain', { domain })
        }
        const proxy = await this.proxyRedisService.findByIndex(proxyIndex)
        proxy.proxyIndex = proxyIndex
        if (!proxy) {
          throw new Q.Errors.NoProxyAvailableError('Not able to find proxy for domain', { domain })
        }
        if (proxy && proxy.unlimited) {
          return proxy
        }
        await this.domainRedisService.updateTimestamp(domain, proxyIndex)
        return proxy
      }

      async tryToCreateProxySet() {
        const proxySetAvailable = await this.proxyRedisService.exists()
        if (proxySetAvailable) return
        let lock
        try {
          lock = await this.lockFactory.acquire('proxy:createIndexSet:lock', { timeout: 20000, retries: 20, delay: 1000 })
          if (!lock) return
          const proxies = await this.providerService.findProxies(1, 1000000)

          if (!await this.proxyRedisService.exists()) {
            Q.log.info({ proxies: proxies.length }, 'Creating index set of proxies in Redis')
            await this.proxyRedisService.create(proxies)
          }
        } finally {
          if (lock) {
            await lock.release().catch(Error, err => {
              Q.log.warn({ err }, 'unable to release lock')
            })
          }
        }
      }

      async tryToCreateDefaultDomainSet() {
        const defaultDomainExists = await this.domainRedisService.domainExists('default')
        if (!defaultDomainExists) {
          await this.tryToCreateDomain('default')
        }
      }

      async tryToCreateDomain(domain) {
        const foundPolicy = await this.tryToCreateAndGetPolicy(domain)
        if (!foundPolicy) {
          await this.domainRedisService.cloneDomain(domain, 'default')
          return
        }
        let lock
        try {
          lock = await this.lockFactory.acquire(`proxy:${domain}:lock`, { timeout: 20000, retries: 20, delay: 1000 })
          if (!lock) return
          Q.log.info({ domain }, 'Trying to insert proxies into Redis')
          const maxProxies = 1000000
          const proxies = await this.proxyPolicyMongoService.findByDomain(domain, 1, maxProxies)
          if (_.isEmpty(proxies)) {
            Q.log.info({ domain }, 'No proxies found and domain set wont be created in Redis')
            return
          }
          Q.log.info({ domain, proxies: proxies.length }, 'Proxies found and domain set will be created in Redis')
          await this.domainRedisService.insertProxies(domain, proxies)
        } finally {
          if (lock) {
            await lock.release().catch(Error, err => {
              Q.log.warn({ err }, 'unable to release lock')
            })
          }
        }
      }

      async tryToCreateAndGetPolicy(domain) {
        const policyFromRedis = await this.policyRedisService.findPolicy(domain)
        if (policyFromRedis) {
          return policyFromRedis
        }

        const policyFromMongo = await this.policyMongoService.findOneByDomain(domain)
        if (policyFromMongo) {
          await this.policyRedisService.createPolicy(policyFromMongo)
          return policyFromMongo
        }

        const defaultPolicyFromMongo = await this.policyMongoService.findOneByDomain('default')
        if (defaultPolicyFromMongo) {
          await this.policyRedisService.createPolicy(defaultPolicyFromMongo)
          return defaultPolicyFromMongo
        }
      }

      async removeAllDomains() {
        await this.domainRedisService.removeAll()
      }
    }

First indicator that something is wrong is number of parameters in the constructor. Many parameters in the construtor tells us there are too many responsibilities in this class. We need to solve it by moving the code somewhere else. But how do we know what code is supposed to be moved and where?

WIP: Dec 5 2017

