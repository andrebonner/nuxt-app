<template>
  <div>
    <h3>{{ poll.topic }}</h3>

    <div class="poll__choice--list">
      <div
        class="poll__choice--container"
        v-for="choice in poll.choices"
        :key="choice.id"
        @click="selectChoice(choice)"
      >
        <div class="poll__choice--box" :class="selectedChoiceClass(choice)">
          <span>Select {{ choice.text }} </span>
          <span>({{ choice.count }} votes)</span>
        </div>
      </div>
    </div>

    <div v-if="selectedChoiceId > 0" class="poll__vote">
      <textarea
        v-model="comment"
        placeholder="Enter an optional comment here"
      ></textarea>
      <button @click="voteChoice()">Vote!</button>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from "nuxt-property-decorator";

import { Poll, Choice, ChoiceVote } from "@/lib/polls/models";
import { pollsModule } from "@/store/polls/const";

@Component({})
export default class PollList extends Vue {
  /**
   * Optional comment
   */
  private comment: string = "";
  /**
   * Avoid undefined to make it reactive
   */
  private selectedChoiceId: number = -1;

  @Prop({ type: Object })
  public poll!: Poll;

  @pollsModule.Action("vote")
  private vote!: (choiceVote: ChoiceVote) => void;
  public selectedChoiceClass(choice: Choice): void {}
  public selectChoice(choice: Choice): void {
    this.selectedChoiceId = choice.id;
  }

  public voteChoice(): void {
    console.log("Voting: ", {
      choiceId: this.selectedChoiceId,
      comment: this.comment.length > 0 ? this.comment : undefined,
    });

    this.vote({
      choiceId: this.selectedChoiceId,
      comment: this.comment.length > 0 ? this.comment : undefined,
    });
    // reset vote selection
    this.selectedChoiceId = -1;
    this.comment = "";
  }
}
</script>
<style lang="scss">
.poll__choice--list {
  display: flex;
  flex-flow: row wrap;
}

.poll__choice--container {
  width: 100%;
  padding: 0.5rem;
  box-sizing: border-box;
  @include gt-sm {
    width: 25%;
  }
}

.poll__choice--box {
  @extend %z-depth-1;
  background-color: $colorPrimary;
  color: #ddd;
  padding: 1rem;
  border: 1px solid $colorPrimaryDark;
  border-radius: 4px;
  transition: background-color 0.2s, color 0.2s;

  span {
    display: block;
    text-align: center;
  }
  &:hover {
    cursor: pointer;
    @extend %z-depth-2;
  }

  &.selected {
    @extend %z-depth-3;
    background-color: $colorPrimaryDark;
    color: #fff;
  }
}

.poll__vote {
  textarea {
    width: calc(100% - 1rem);
    margin: 0px 0.5rem;
    padding: 0.5rem;
    box-sizing: border-box;
    resize: vertical;
  }
  button {
    display: block;
    margin: 1rem auto;
    width: 50%;
    max-width: 300px;
    padding: 8px 0px;
    background-color: $colorSecondary;
    color: #ddd;
    border: 1px solid $colorSecondaryDark;
    border-radius: 4px;
    transition: background-color 0.2s, color 0.2s;

    &:hover {
      cursor: pointer;
      background-color: $colorSecondaryDark;
      color: #fff;
    }
  }
}
</style>
