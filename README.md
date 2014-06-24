tag-bundle
==========

This bundle requires FPN\TagBundle

AppKernel
=========
            new FPN\TagBundle\FPNTagBundle(),
            new Vivait\TagBundle\VivaitTagBundle(),

Composer.json
=============
"require": {
    //...
    "vivait/tag-bundle":"dev-master"
    },
    
Usage:
Entities need to implement Vivait/Entity/Taggable and the following methods
```

    public function getTags()
    {
        $this->tags = $this->tags ?: new ArrayCollection();
        return $this->tags;
    }

    public function getTaggableType()
    {
        return 'finance_tag';
    }

    public function getTaggableId()
    {
        return $this->getId();
    }

```

To use you must first init the tagging manager:
```
$tagManager = $this->get('fpn_tag.tag_manager');
$tagManager->loadTagging($entity);
```

When adding a new tag - be aware that you can't add duplicates without Doctrine kicking off about a duplicate key
```
        $tag = $tm->loadOrCreateTag($this->tag);

        if(!$finance->getTags()->contains($tag)) {
            $tm->addTag($tag,$finance);
        }

        $em->persist($finance);
        $em->flush();

        $tm->saveTagging($finance);
```

When removing a tag - you must still load the tagging manager
```
    $tm = $this->tag_manager;
    $em = $this->em;

        $tm->loadTagging($finance);
        $tag = $tm->loadOrCreateTag($this->tag);

        $tm->removeTag($tag, $finance);
        $em->persist($finance);
        $em->flush();

        $tm->saveTagging($finance);


```
