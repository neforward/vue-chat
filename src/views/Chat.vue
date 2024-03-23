<template>
  <div class="wrapper">
    <div class="people">
      <header>Your Messages</header>
      <ol class="people-list">
        <li v-for="(user, index) in userData" :key="index">
          <div
            class="people-item"
            :class="{ active: selectedUser === user.userId }"
            @click="switchUser(user.userId)"
          >
            <div class="people-pfp">
              <img :src="user.profilePicture" alt="" />
            </div>
            <div>
              <h2>{{ user.name }}</h2>
              <span>{{ user.status }}</span>
            </div>
          </div>
        </li>
      </ol>
      <div class="user-selection">
        <h2>Select User:</h2>
        <button
          v-for="(user, index) in userData"
          :key="index"
          @click="switchUser(user.userId)"
        >
          {{ user.name }}
        </button>
      </div>
    </div>
    <div v-if="selectedUser" class="chat">
      <header>
        <div class="chat-info">
          <div class="chat-avatar">
            <img :src="selectedUserProfilePicture" alt="ava" />
          </div>
          <div>
            <h2>{{ selectedUserName }}</h2>
            <span>{{ selectedUserStatus }}</span>
          </div>
        </div>
        <svg
          width="24"
          height="24"
          viewBox="0 0 24 24"
          fill="none"
          xmlns="http://www.w3.org/2000/svg"
        >
          <path
            d="M12 14C13.1046 14 14 13.1046 14 12C14 10.8954 13.1046 10 12 10C10.8954 10 10 10.8954 10 12C10 13.1046 10.8954 14 12 14Z"
            fill="white"
          />
          <path
            d="M12 7C13.1046 7 14 6.10457 14 5C14 3.89543 13.1046 3 12 3C10.8954 3 10 3.89543 10 5C10 6.10457 10.8954 7 12 7Z"
            fill="white"
          />
          <path
            d="M12 21C13.1046 21 14 20.1046 14 19C14 17.8954 13.1046 17 12 17C10.8954 17 10 17.8954 10 19C10 20.1046 10.8954 21 12 21Z"
            fill="white"
          />
        </svg>
      </header>
      <ol class="messages">
        <li
          v-for="(message, index) in filteredMessages"
          :key="index"
          :class="{ me: message.userId === selectedUser }"
        >
          <div class="message-text">{{ message.text }}</div>
          <div class="message-time">{{ message.time }}</div>
        </li>
      </ol>
      <label>
        <textarea
          type="text"
          placeholder="Type your message..."
          v-model="newMessageText"
          @keyup.enter="sendMessage"
        ></textarea>
        <div class="send" @click="sendMessage">Send</div>
      </label>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, computed } from "vue";

const userData = [
  {
    userId: "neforward",
    name: "Travis Scott",
    profilePicture:
      "https://is1-ssl.mzstatic.com/image/thumb/Music116/v4/75/8e/5f/758e5fae-251f-6330-c663-5be28960f0d3/886445454987.jpg/600x600bf-60.jpg",
    status: "online",
  },
  {
    userId: "companion",
    name: "Lil Uzi Vert",
    profilePicture: "https://iscale.iheart.com/catalog/artist/31083712",
    status: "online",
  },
  {
    userId: "newOne",
    name: "Playboi Carti",
    profilePicture:
      "https://i1.sndcdn.com/artworks-kpGMczb3oHz2j2cg-Cir1uA-t500x500.jpg",
    status: "online",
  },
];

const messageData = [
  { userId: "neforward", text: "" },
  { userId: "companion", text: "" },
  { userId: "newOne", text: "" },
];

const selectedUser = ref(null);
const newMessageText = ref("");
const messages = ref([]);

const switchUser = (userId) => {
  selectedUser.value = userId;
};

const filteredMessages = computed(() => {
  return messages.value.filter(
    (message) => message.userId === selectedUser.value
  );
});

const generateMessage = () => {
  const randomIndex = Math.floor(Math.random() * messageData.length);
  const newMessage = {
    ...messageData[randomIndex],
    time: new Date().toLocaleTimeString(),
    userId: selectedUser.value,
  };
  messages.value.push(newMessage);
};

const sendMessage = () => {
  if (newMessageText.value.trim() !== "") {
    const newMessage = {
      userId: selectedUser.value,
      text: newMessageText.value,
      time: new Date().toLocaleTimeString(),
    };
    messages.value.push(newMessage);
    newMessageText.value = "";
    generateMessage();
  }
};
const handleEnterKey = (event) => {
  if (event.key === "Enter") {
    sendMessage();
  }
};

onMounted(() => {
  messageData.forEach((message) => messages.value.push(message));
});

onMounted(() => {
  generateMessage();
});

const selectedUserProfilePicture = computed(() => {
  const user = userData.find((user) => user.userId === selectedUser.value);
  return user ? user.profilePicture : "";
});

const selectedUserName = computed(() => {
  const user = userData.find((user) => user.userId === selectedUser.value);
  return user ? user.name : "";
});

const selectedUserStatus = computed(() => {
  const user = userData.find((user) => user.userId === selectedUser.value);
  return user ? user.status : "";
});
</script>

<style lang="scss">
.people-list {
  cursor: pointer;
}
.chat {
  flex: 1;

  header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    background: #3f5984;
    padding: 16px;

    svg {
      cursor: pointer;
    }

    .chat-avatar img {
      cursor: pointer;
      width: 60px;
      height: 60px;
    }
  }

  &-info {
    display: flex;
    align-items: center;
    gap: 16px;

    h2 {
      color: #f6f6f6;
      font-size: 16px;
      font-weight: 500;
    }

    span {
      color: #1baa75;
      font-size: 14px;
      font-weight: 500;
    }
  }

  li {
    margin-bottom: 11px;
  }
  .message.me {
    background: #1baa75;
    max-width: 45%;
    margin-left: auto;
    color: #f6f6f6;
    font-size: 14px;
    line-height: 140%;

    .message-text {
      padding: 15px 15px 0 15px;
    }

    .message-time {
      font-size: 12px;
      display: flex;
      align-items: center;
      justify-content: flex-end;
      gap: 5px;
      padding: 5px;
    }
  }

  select {
    margin-bottom: 5px;
  }
  .message {
    background: #efefef;
    max-width: 50%;
    color: #24334b;
    font-size: 14px;
    line-height: 140%;
    margin-right: auto;

    .message-text {
      padding: 15px 15px 0 15px;
    }

    .message-time {
      font-size: 12px;
      display: flex;
      align-items: center;
      justify-content: flex-end;
      gap: 5px;
    }
    &.me {
      background: #1baa75;
      max-width: 45%;
      margin-left: auto;
      color: #f6f6f6;
      font-size: 14px;
      line-height: 140%;
      margin-right: 0;

      .message-text {
        padding: 15px 15px 0 15px;
      }

      .message-time {
        font-size: 12px;
        display: flex;
        align-items: center;
        justify-content: flex-end;
        gap: 5px;
        padding: 5px;

      }
    }
  }

  label {
    display: flex;
    align-items: center;

    textarea {
      flex: 1;
      resize: none;
      padding: 22px 15px;
      box-sizing: border-box;
      height: 64px;
      font-family: "Montserrat";
    }

    .send {
      padding: 15px 25px;
      background: #1baa75;
      cursor: pointer;
    }
  }
}
</style>
