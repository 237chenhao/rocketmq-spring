> 1.修改rocketmq-spring-boot-parent模块的pom.xml
        <spring.boot.version>1.5.13.RELEASE</spring.boot.version>
        <spring.version>4.3.17.RELEASE</spring.version>
        
        

> 2.修改ListenerContainerConfiguration类的方法registerContainer中的部分代码
            RocketMQMessageListener annotation = clazz.getAnnotation(RocketMQMessageListener.class);
            validate(annotation);
    
            BeanDefinitionBuilder beanDefinitionBuilder = BeanDefinitionBuilder.rootBeanDefinition(DefaultRocketMQListenerContainer.class);
            beanDefinitionBuilder.addPropertyValue("nameServer", rocketMQProperties.getNameServer());
            beanDefinitionBuilder.addPropertyValue("topic", environment.resolvePlaceholders(annotation.topic()));
            beanDefinitionBuilder.addPropertyValue("consumerGroup", environment.resolvePlaceholders(annotation.consumerGroup()));
            beanDefinitionBuilder.addPropertyValue("objectMapper", objectMapper);
            beanDefinitionBuilder.addPropertyValue("rocketMQListener", bean);
            beanDefinitionBuilder.addPropertyValue("rocketMQMessageListener", annotation);
            beanDefinitionBuilder.setInitMethodName("start");
    
            AbstractBeanDefinition beanDefinition = beanDefinitionBuilder.getBeanDefinition();
    
            String containerBeanName = String.format("%s_%s", DefaultRocketMQListenerContainer.class.getName(),
                counter.incrementAndGet());
            GenericApplicationContext genericApplicationContext = (GenericApplicationContext) applicationContext;
            genericApplicationContext.registerBeanDefinition(containerBeanName, beanDefinition);
    
    //        genericApplicationContext.registerBean(containerBeanName, DefaultRocketMQListenerContainer.class,
    //            () -> {
    //                System.out.println("=============**************************=======");
    //                return createRocketMQListenerContainer(bean, annotation);
    //            });
    //        DefaultRocketMQListenerContainer container = genericApplicationContext.getBean(containerBeanName,
    //            DefaultRocketMQListenerContainer.class);
    //        if (!container.isRunning()) {
    //            try {
    //                container.start();
    //            } catch (Exception e) {
    //                log.error("Started container failed. {}", container, e);
    //                throw new RuntimeException(e);
    //            }
    //        }
    
            log.info("Register the listener to container, listenerBeanName:{}, containerBeanName:{}", beanName, containerBeanName);
> 3.OK
